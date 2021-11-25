# 温故知新，Vue 指令之 v-for

## 缘起

最近 Review 同事的代码，有一处使用了 `v-for` 进行对象遍历，看的时候下意识的认知是 JS for in 遍历的是对象的键，所以觉得有点问题，然后又突然反应了过来 `v-for` 遍历的是对象的属性值，实际上并不是 for in。

倒是感觉这个设计有一点反直觉了，当然可能也跟我自己很少会拿对象进行 `v-for` 操作有关。

## v-for

关于 `v-for` 的使用，[官网教程的列表渲染章节](https://cn.vuejs.org/v2/guide/list.html)已经介绍地很详细了，不再赘述。

## 实现

翻了一下 Vue 2.x 源码中 `v-for` 的实现，可以从两个方面来看，一是 template 的指令解析，二是具体的 render。

我们知道 Vue 会将 template 解析为 AST，在源码的 `src/compiler/index.js` 作为入口有这个调用 `const ast = parse(template.trim(), options)`。

跟踪 `parse` 方法到 `src/compiler/parser/index`，`parse` 方法返回的解析的 AST 对象，定义类型为 ASTElement，可以在 `flow/compiler.js` 中找到，它上面定义了 `attrsMap` 和 `attrsList` 两个属性。

```js
/**
 * Convert HTML string to AST.
 */
export function parse (
  template: string,
  options: CompilerOptions
): ASTElement | void {}
```

继续跟踪可以找到 `processFor` 和 `parseFor` 两个方法，整体调用链是 `parseHTML => parseHTML.options.start => processFor => parseFor`。

`parseHTML` 方法的具体实现细节太多略过不提，只是在 `start` 方法的调用时关注传入的 `attrs` 参数，在 `src/compiler/parser/html-parser.js` 中，这个参数是 HTML 属性的解析结果，大致是 `{ name: '', value: '' }` 这样的形式，当然还包含其他键值。

传入的 `attrs` 参数会在 `start` 方法的处理中通过 `createASTElement` 方法得到前面提到的 ASTElement 类型，然后调用 `processFor` 传入 ASTElement。

下面是 `processFor` 和 `parseFor` 的具体实现。

```js
export const forAliasRE = /([\s\S]*?)\s+(?:in|of)\s+([\s\S]*)/
export const forIteratorRE = /,([^,\}\]]*)(?:,([^,\}\]]*))?$/
const stripParensRE = /^\(|\)$/g
// ...
export function processFor (el: ASTElement) {
  let exp
  if ((exp = getAndRemoveAttr(el, 'v-for'))) {
    const res = parseFor(exp)
    if (res) {
      extend(el, res) // extend 方法将 res 上的属性合并到 el 上
    } else if (process.env.NODE_ENV !== 'production') {
      warn(
        `Invalid v-for expression: ${exp}`,
        el.rawAttrsMap['v-for']
      )
    }
  }
}

type ForParseResult = {
  for: string;
  alias: string;
  iterator1?: string;
  iterator2?: string;
};

export function parseFor (exp: string): ?ForParseResult {
  const inMatch = exp.match(forAliasRE)
  if (!inMatch) return
  const res = {}
  res.for = inMatch[2].trim()
  const alias = inMatch[1].trim().replace(stripParensRE, '')
  const iteratorMatch = alias.match(forIteratorRE)
  if (iteratorMatch) {
    res.alias = alias.replace(forIteratorRE, '').trim()
    res.iterator1 = iteratorMatch[1].trim()
    if (iteratorMatch[2]) {
      res.iterator2 = iteratorMatch[2].trim()
    }
  } else {
    res.alias = alias
  }
  return res
}
```

`getAndRemoveAttr` 的实现在 `src/compiler/helper.js` 中，就是从 `attrsMap` 中取出对应属性的值。

```js
// note: this only removes the attr from the Array (attrsList) so that it
// doesn't get processed by processAttrs.
// By default it does NOT remove it from the map (attrsMap) because the map is
// needed during codegen.
export function getAndRemoveAttr (
  el: ASTElement,
  name: string,
  removeFromMap?: boolean
): ?string {
  let val
  if ((val = el.attrsMap[name]) != null) {
    const list = el.attrsList
    for (let i = 0, l = list.length; i < l; i++) {
      if (list[i].name === name) {
        list.splice(i, 1)
        break
      }
    }
  }
  if (removeFromMap) {
    delete el.attrsMap[name]
  }
  return val
}
```

举个例子，假设我们写了这一段 `v-for` 的代码，`<li v-for="item in items">`，那么通过 `parseFor` 处理之后大概会得到这样一个对象 `{ for: 'items', alias: 'item' }`，然后这个属性会拓展到 ASTElement 上。

看到这里，如何处理 `v-for` 指令到 AST 对象差不多就清楚了，继续回到 `src/compiler/index.js` 会将处理之后的 AST 对象用于生成 JavaScript 代码，`const code = generate(ast, options)`，具体实现在 `src/compiler/codegen/index.js` 中。

这里同样有太多细节，大致调用是 `generate => genElement => genFor`。

```js
export function genFor (
  el: any,
  state: CodegenState,
  altGen?: Function,
  altHelper?: string
): string {
  const exp = el.for
  const alias = el.alias
  const iterator1 = el.iterator1 ? `,${el.iterator1}` : ''
  const iterator2 = el.iterator2 ? `,${el.iterator2}` : ''

  // ...

  el.forProcessed = true // avoid recursion
  return `${altHelper || '_l'}((${exp}),` +
    `function(${alias}${iterator1}${iterator2}){` +
      `return ${(altGen || genElement)(el, state)}` +
    '})'
}
```

`genFor` 主要关注最后的字符串拼接，特别是 `return ${(altGen || genElement)(el, state)}` 这里，altGen 忽略，这里实际上是 `${genElement(el, state)}`，又进行了一次 genElement 调用，所以上面有 `el.forProcessed = true` 避免无限递归。

`iterator1` 和 `iterator2` 分别是使用 `v-for` 时的后两个参数，比如 `v-for="(value, key, index) in object"`。

所以 `genFor` 最后返回的大概是这样的伪代码：

```js
_l((items), function(item) {
  return [code string returned from genElement(el, state)]
})
```

是不是有点 render function 的样子了，以上是 template 的指令解析，下面 render 的关键逻辑就简单一点。


`_l` 方法在 `src/core/instance/render-helpers/index.js` 里注入，可以找到 `src/core/instance/render-helpers/render-list.js` 的 `renderList` 方法。

```js
/**
 * Runtime helper for rendering v-for lists.
 */
export function renderList (
  val: any,
  render: (
    val: any,
    keyOrIndex: string | number,
    index?: number
  ) => VNode
): ?Array<VNode> {
  let ret: ?Array<VNode>, i, l, keys, key
  // 首先判断数组或字符串遍历
  if (Array.isArray(val) || typeof val === 'string') {
    ret = new Array(val.length)
    for (i = 0, l = val.length; i < l; i++) {
      ret[i] = render(val[i], i)
    }
  } else if (typeof val === 'number') {
  // 其次判断是数字的遍历
    ret = new Array(val)
    for (i = 0; i < val; i++) {
      ret[i] = render(i + 1, i)
    }
  } else if (isObject(val)) {
  // isObject 是宽泛判断，调用时潜在确认一定是 json object
  // 最后是对象遍历，有两种情况
    if (hasSymbol && val[Symbol.iterator]) {
      // 对象具有迭代器属性会优先用迭代器进行遍历
      ret = []
      const iterator: Iterator<any> = val[Symbol.iterator]()
      let result = iterator.next()
      while (!result.done) {
        ret.push(render(result.value, ret.length))
        result = iterator.next()
      }
    } else {
      // 普通对象会遍历 Object.keys()
      keys = Object.keys(val)
      ret = new Array(keys.length)
      for (i = 0, l = keys.length; i < l; i++) {
        key = keys[i]
        // 只有没有迭代器的对象才支持 val, key, index 三个参数
        ret[i] = render(val[key], key, i)
      }
    }
  }
  if (!isDef(ret)) {
    ret = []
  }
  (ret: any)._isVList = true
  return ret
}
```

以上是 `v-for` 在源码中的一个大致实现过程，其实还有两个问题，一是 `renderList` 的第二个参数 `render` 函数的 `keyOrIndex` 这个参数是必须传入的，在具体调用时也都有传入，但是生成的 `_l` 代码中有可能是不会传入 `iterator1` 的；另一个问题是 `v-for` 使用时必须同时绑定 `key` 属性，以便对 vdom diff 进行优化，在这个流程上其实没有看到对应的处理。

前者应该是类似形参实参这样声明代码和调用代码的关系，后者多半是具体 render 过程中才会涉及的处理，本文就不细究了。
