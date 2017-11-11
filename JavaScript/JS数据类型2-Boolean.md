# JS数据类型2 - Boolean


Boolean只有两个值，`true`和`false`，在JS中大小写敏感，即`True`和`False`并不是布尔值

程序中往往会将1当做true，0当做false，但布尔值本身和这两个数值是无关的，true不一定等于1，false也不一定等于0

JS中所有类型的值都可以通过调用`Boolean()`函数转换为其对应的布尔值，因此需要特别注意作为原始类型的布尔值和其他可以通过调用`Boolean()`方法转为布尔值的类型之间的区分关系，这种转换在`if`判断中会被自动执行

具体转换规则如下

```js
// String
Boolean('false') 
Boolean('abc')                          // 非空字符串 => true
Boolean('')                             // 空字符串 => false

// Number
Boolean(123)
Boolean(-100.12)
Boolean(Infinity)
Boolean(-Infinity)                      // 非零数值和无穷大 => true
Boolean(0)
Boolean(-0)
Boolean(NaN)                            // 0和NaN => false

// Symbol
Boolean(Symbol())                       // Symbol() => true

// Object
Boolean({})
Boolean([])
Boolean(function() { return false })    // 任何对象 => true
Boolean(null)                           // null => false

// Undefined
Boolean(undefined)                      // undefined => false
```

对于`Boolean()`函数直接调用会返回一个布尔值，而通过`new`的形式调用则会返回一个构建的布尔对象，既然是一个对象，那么在任何时候，这个布尔对象值转换到布尔值都是true，即使new了一个false的布尔对象

换言之，如果我们在`if`语句中用`new Boolean()`返回的布尔对象，那么程序将会当做条件为true来执行

```js
if (new Boolean(false)) {
  alert(1)
}
```

此外还要注意，新手常常会将其他类型与布尔类型之间的值转换规则套用到相等判断`==`上面去，这在某些特殊比较上往往会引起困惑，其实js的`==`判断另有其比较规则，这里就不展开了，另外写文总结吧


参考资料
- 《JavaScript高级程序设计》（第三版）