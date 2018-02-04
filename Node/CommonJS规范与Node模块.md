# CommonJS规范与Node模块

JS在ES6标准之前，是没有自己的模块机制的。早期的JS开发，由于主要运行在浏览器端，通过不同script脚本的引入，以达到某种程度上的“模块化”。然而由于浏览器对脚本加载的机制，这样的方式也带来了一些诸如全局变量污染，脚本阻塞，依赖关系不明确，应用不够健壮，代码可维护性低等等问题。

面对这些问题，社区也不断总结出了一些最佳实践。比如script脚本放置在body闭合标签之前；比如通过人为的约定来规范全局变量，例如引入命名空间的概念;再比如通过IIFE（匿名立即执行函数）来实现代码的模块化等等。

再之后，社区也陆续推出了一些约定和规范，形成了像AMD，CMD，CommonJS这些JS的模块化方案。基于这些规范，也出现了像RequireJS，Sea.js，这些模块加载器，作为对规范的实现。

那么Node.js遵循的是CommonJS规范，Node基于这个规范实现了一套模块加载机制，而基于Node下面开发的各种应用，也应该是遵循CommonJS规范的。

## CommonJS模块规范

### 对于模块主要遵循三个约定

1. require函数

2. 模块上下文

3. 模块标识

#### require函数

接受一个字符串参数，作为所引用模块的模块名（或者说，模块标识）

返回值就是所引用的模块所暴露出来的API

实现一套模块化规范需要实现模块的加载器，可以粗浅地将加载器理解为require，加载器在背后做了许多事情，最后通过require暴露出来

```js
const add = require('add')
// 通过add就可以访问到'add'模块暴露出来的api
```

#### 模块上下文

满足以下三个条件，即可称为一个模块上下文

1. require函数

2. exports对象

3. module对象

module对象包含了模块自身的一些东西，如只读的`module.id`

其中最重要的就是模块暴露出去的一个值`module.exports`, 默认情况下这个值和exports对象指向同一个指针

所以可以通过`exports.xxx = xxx`的形式来暴露模块的api

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

但是需要注意的是，真正暴露出去的其实是`module.exports`，如果这个值被替换了，那么再通过`exports.xxx = xxx`就无法暴露出去了，因为不是指向同一个指针

```js
// module.js
const l = console.log.bind(console)
l(module)
l(exports) // 初始是一个空对象
exports.a = 'a'
l(exports === module.exports)
module.exports = 'abc' // 模块暴露出去的值就是abc
l(exports !== module.exports)
// index.js
const m = require('module.js')
// module.exports实际上就是require的返回值
```

实际上，每个js文件或者模块，都被当作是一个闭包，如果不去修改global等全局变量，那么就不会污染全局

#### 模块标识

通俗地说，就是模块名，作为一个字符串传给require函数

按照CommonJS规范，模块标识需要遵循如下格式：

1. 小驼峰格式的标识名

2. 以'.'或者'..'开头的相对路径

3. 不应该带上后缀名，如'.js'

> Module Identifiers

> A module identifier is a String of "terms" delimited by forward slashes.

> A term must be a camelCase identifier, ".", or "..".

> Module identifiers may not have file-name extensions like ".js".

> Module identifiers may be "relative" or "top-level". A module identifier is "relative" if the first term is "." or "..".

> Top-level identifiers are resolved off the conceptual module name space root.

> Relative identifiers are resolved relative to the identifier of the module in which "require" is written and called.

但是在nodejs中，实际上都没有遵循这些命名规范，比如很多包名都是中划线命名，所以也就入乡随俗吧

#### 两个没有指明的约定

一个模块只要遵循上面的三个约定，就可以称作CommonJS模块

1. 存储方案

nodejs为例，模块的存储方案就是文件系统（c++模块除外，动态链接库）

2. 加载器可以支持环境变量寻径，也可以不支持

这两个约定可以实现，也可以不实现，不影响对CommonJS规范的实现

## Node模块

一个Node应用从入口js文件开始，内部由一个个node模块组成

一个node模块基本上就是遵循前面三个约定的CommonJS模块或者'.node'的c++二进制模块，通过require函数引入程序

### nodejs模块寻径

1. Node.js核心模块

核心模块的代码存在于Node.js源码库中，编译到Node.js可执行文件里，通过预留的模块标识将核心模块的API暴露出来

```js
const fs = require('fs')
// 模块标识符合核心模块时，寻径直接返回核心模块
```

2. 文件模块

通过文件寻径的方式来查找，包括三方模块和项目模块

如果通过寻径查找到是一个目录，那么Node.js就会依次寻找目录下的`index.js`, `index.json`和`index.node`文件，如果找到就返回，没有就报错

- 三方模块



- 项目模块










