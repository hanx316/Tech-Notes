# JS数据类型1 - undefined和null


ES6以前，JS有五种基本数据类型（又叫原始类型，primitive），undefined、null、Boolean、Number和String，以及一种复杂数据类型（complex），Object

ES6新增了一种新的基本数据类型，Symbol

目前，JS共计有七种数据类型，六个基本类型，一个复杂类型

## Undefined

Undefined类型的值为undefined

常见就是：
- 变量声明之后但未赋值，此时变量的值是undefined
- 访问了对象不存在的属性会得到undefined的值
- 函数调用时没有传入实参，访问形参的值是undefined
- 函数没有返回值时，默认返回undefined

```js
var msg
var obj = {}
console.log(msg)            // undefined
console.log(obj.a)          // undefined
undefined == undefined      // true
undefined === undefined     // true
```

**需要注意typeof操作符**

当对一个未被声明过的变量执行typeof操作符也会返回undefined

对于let和const声明，由于不会存在声明提升以及TDZ的存在，对当前作用域内在let和const声明之前的变量进行typeof操作符会报错

```js
typeof a                    // "undefined"
typeof b                    // Uncaught ReferenceError: b is not defined
let b
```

此外，在ES3时，window.undefined是一个可以改变的普通值，所以在当时利用undefined来进行判断会存在一些潜在的风险

## Null

Null类型的值为null

null表示一个空对象指针，`typeof null === 'Object' // true`

编程语言中都有对于空值，即没有值的表示
> 例如，Java也用null，Swift用nil，Python用None表示

JS中存在undefined和null，前者表示未定义，后者表示空值。这常常让人觉得困惑，但其实是最初设计者的一个“错误”而沿用了下来。
> 根据C语言的传统，null被设计成可以自动转为0
> null像在Java里一样，被当成一个对象，Brendan Eich觉得表示"无"的值最好不是对象
> JavaScript的最初版本没有包括错误处理机制，发生数据类型不匹配时，往往是自动转换类型或者默默地失败。Brendan Eich觉得，如果null自动转为0，很不容易发现错误
> 因此，Brendan Eich又设计了一个undefined

undefined派生自null，所以ECMA-262规定 `null == undefined // true`

一般而言，当在初始化一个变量时，如果明确将来这个变量要保存的值是Object类型，那么应该初始化为null

DOM操作时，如果获取DOM中不存在的节点，将会返回null，例如`document.querySelector('#notExistDOM')`

对象原型链的终点也是null，`Object.getPrototypeOf(Object.prototype) // null`

## Undefined和Null的异同

有的面试题会有这样的问题，所以也特别总结一下

除了前面提到的两者定义和区别之外还有以下异同

相同之处：
- 都是基本（原始）类型
- 逻辑上，都表示false

不同之处：
- typeof操作符返回值
```js
typeof null         // "Object"
typeof undefined    // "undefined"
```
- Number转换
```js
Number(null)        // 0
Number(undefined)   // NaN
```
- null是一个合法有效的JSON，而undefined不是


参考资料
- 《JavaScript高级程序设计》（第三版）
- [undefined与null的区别 - 阮一峰](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)