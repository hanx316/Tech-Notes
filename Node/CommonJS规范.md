# CommonJS 规范

JS 在 ES6 标准之前，是没有自己的模块机制的。早期的 JS 开发，由于主要运行在浏览器端，通过不同 script 脚本的引入，以达到某种程度上的“模块化”。然而由于浏览器对脚本加载的机制，这样的方式也带来了一些诸如全局变量污染，脚本阻塞，依赖关系不明确，应用不够健壮，代码可维护性低等等问题。

面对这些问题，社区也不断总结出了一些最佳实践。比如 script 脚本放置在 body 闭合标签之前；比如通过人为的约定来规范全局变量，例如引入命名空间的概念；再比如通过 IIFE（匿名立即执行函数）来实现代码的模块化等等。

再之后，社区也陆续推出了一些约定和规范，形成了像 AMD，CMD，CommonJS 这些 JS 的模块化方案。基于这些规范，也出现了像 RequireJS，Sea.js，这些模块加载器，作为对规范的实现。

Node.js 遵循的是 CommonJS 规范，Node 基于这个规范实现了一套模块加载机制，而基于 Node 开发的各种应用，也应该是遵循 CommonJS 规范的。

## CommonJS 模块规范

一个 CommonJS 的模块主要遵循以下三个约定：

1. require函数

2. 模块上下文

3. 模块标识

### require函数

接受一个字符串参数，作为所引用模块的模块名（或者说，模块标识）。

返回值就是所引用的模块所暴露出来的API。

实现一套模块化规范需要实现模块的加载器，可以粗浅地将加载器理解为 require，加载器在背后做了许多事情，最后通过 require 暴露出来。

```js
const add = require('add')
// 通过add就可以访问到'add'模块暴露出来的api
```

### 模块上下文

满足以下三个条件，即可称为一个 CommonJS 模块的上下文：

1. require 函数（前面提到的）

2. exports 对象

3. module 对象

module 对象包含了模块自身的一些东西，如只读的 `module.id`，其中最重要的就是模块暴露出去的一个值 `module.exports`，默认情况下这个值和 exports 对象指向同一个指针。

所以可以通过 `exports.xxx = xxx` 的形式来暴露模块的 api。

再强调一遍，模块实际暴露出去的是 `module.exports` 而非 `exports` 对象，只有这两者共享同一个指针，所以在没有改变 module.exports 引用的情况下可以通过 exports 来暴露 api。

```js
// module calculate
exports.plus = (x, y) => x + y
exports.minus = (x, y) => x - y
console.log(exports === module.exports) // true
// index
const calculate = require('calculate')
calculate.plus(1, 2)
calculate.minus(3, 1)
```

真正暴露出去的其实是 `module.exports`，如果这个值被替换了，那么再通过 `exports.xxx = xxx` 就无法暴露出去了，因为不是指向同一个指针。

```js
// module.js
const l = console.log.bind(console)
l(module)
l(exports) // 初始是一个空对象
exports.a = 'a'
l(exports === module.exports)
module.exports = 'abc' // 模块暴露出去的值就是 abc
l(exports !== module.exports)
// index.js
const m = require('module.js')
// module.exports 实际上就是 require 的返回值
```

实际上，每个 js 文件或者模块，都被当作是一个闭包，如果不去修改 global 等全局变量，那么就不会污染全局变量。

### 模块标识

通俗地说，就是模块名，作为一个字符串传给 require 函数

按照 CommonJS 规范，模块标识需要遵循如下格式：

1. 小驼峰格式的标识名

2. 以 '.' 或者 '..' 开头的相对路径

3. 不应该带上后缀名，如 '.js'

> Module Identifiers

> A module identifier is a String of "terms" delimited by forward slashes.

> A term must be a camelCase identifier, ".", or "..".

> Module identifiers may not have file-name extensions like ".js".

> Module identifiers may be "relative" or "top-level". A module identifier is "relative" if the first term is "." or "..".

> Top-level identifiers are resolved off the conceptual module name space root.

> Relative identifiers are resolved relative to the identifier of the module in which "require" is written and called.

但是在 nodejs 中，实际上都没有遵循这些命名规范，比如很多包名都是中划线命名，所以也就入乡随俗吧

### 两个没有指明的约定

一个模块只要遵循上面的三个约定，就可以称作 CommonJS 模块

1. 存储方案

nodejs 为例，模块的存储方案就是文件系统（c++模块除外，动态链接库）

2. 加载器可以支持环境变量寻径，也可以不支持

这两个约定可以实现，也可以不实现，不影响对 CommonJS 规范的实现
