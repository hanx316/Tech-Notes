# JS数据类型3 - Number


对于数字，JS只有Number这一个类型，而不像其他语言会有int和float或者double这样的类型区分

Number类型在JS语言内部以64位浮点数值（双精度数值）来表示，即IEEE754格式

所以在JS中`1`和`1.0`都是同样的值

可以使用JS内置的`Number()`构造函数将**除了Symbol类型之外的值**转为Number类型

## 数值字面量

通常我们都用十进制表示数字，JS中也可以用八进制和十六进制的字面量，在ES6之前八进制的字面量在严格模式下被认为是错误

ES6以后，JS可以分别用`0b`和`0o`开头来表示二进制和八进制的字面量

## 精度问题

对于IEEE754格式保存的数值，最高精度是17位小数，因此在浮点数的计算上会存在舍入导致的误差

例如最经典的`0.1 + 0.2 !== 0.3`问题，不单单是JS有这个问题，[http://0.30000000000000004.com/](http://0.30000000000000004.com/)这个网站专门列出了不少语言的处理结果

关于这个问题，涉及到了计算机内部的二进制编码知识，对于计算机来说，不管你是什么数字，它都只认0和1，因此整数也好，小数也好，都要转成二进制保存在计算机中

但是由于内存有限，计算机并不能保存无限长的二进制位，IEEE754格式保存的数值会对超出范围的二进制数采取舍入模式，如此再进行计算精度也就丢失了

## 数值范围

也因为内存限制和IEEE754格式，JS可以表示的数值会有一个固定的范围

### 最大值和最小值

可以通过`Number.MIN_VALUE`和`Number.MAX_VALUE`访问到当前运行环境下的这两个边界值，在大数值计算中也要注意不要超过数值表示范围

可以用`isFinite()`或者`Number.isFinite()`方法来检测一个值是不是落在这个区间范围内，后者是ES6扩充到Number对象上的方法，前者则是全局对象上的方法

`isFinite()`调用时会先调用`Number()`将一个值转为数字类型而`Number.isFinite()`对非Number类型直接返回false

### 科学计数法

又叫e表示法，一般用来表示特别大的数，一个数字的字面量的值等于e之前的数字乘以10的e之后的数字乘方，例如，100用e表示法则是1e2

```js
Number.MIN_VALUE === 5e-324                     // true
Number.MAX_VALUE === 1.7976931348623157e+308    // true
```
### 最小精度数

ES6在`Number`对象上，新增一个极小的常量`Number.EPSILON`，表示1与大于1的最小浮点数之间的差

它被当做JS能够表示的最小精度，如果误差小于这个值，则可以认为没有误差

### 安全整数

JS能够表示的整数范围在正负2的53次方之间（不含这两个数）

ES6在`Number`对象上引入了`Number.MAX_SAFE_INTEGER`和`Number.MIN_SAFE_INTEGER`来表示JS能够表示的最大整数和最小整数，即

```js
Number.MAX_SAFE_INTEGER === Math.pow(2, 53) - 1         // true
Number.MIN_SAFE_INTEGER === -Math.pow(2, 53) + 1        // true
```

ES6以后可以调用`Number.isInteger()`方法来检测一个值是否是整数，调用`Number.isSafeInteger()`方法检测一个整数是否在安全整数范围内

### 正负无穷和非数值

Number类型有三个特殊的值，`Infinity`和`-Infinity`以及`NaN`，分别表示正无穷，负无穷和非数值

任何涉及到这三个值的Number类型的运算都会返回这个三个值本身

```js
Infinity + 1    // Infinity
-Infinity + 1   // -Infinity
NaN/2           // NaN
```

除数为0时会比较特殊

```js
1/0     // 正整数除以0返回Infinity
-1/0    // 负整数除以0返回-Infinity
0/0     // 0除以0返回NaN
```

`NaN`与任何值都不相等，包括它本身，因此判断一个值是否是`NaN`不能用相等和全等操作符

可以用`isNaN()`或者`Number.isNaN()`方法来检测一个值是不是`NaN`，和`isFinite`一样，后者是ES6扩充到Number对象上的方法，前者则是全局对象上的方法

同样，`isNaN()`调用时会先调用`Number()`将一个值转为数字类型，因此`isNaN('NaN')`将会返回true，因为`Number('NaN')`返回了`NaN`

而`Number.isNaN()`方法对任何不是`NaN`的值一律返回false，所以调用该方法检测`NaN`值更加安全

## 数值转换

### Number()

`Number()`方法可以将除了Symbol以外的任意类型转为对应的数值

```js
// Boolean or new Boolean()
Number(true)                        // 1
Number(false)                       // 0
Number(new Boolean(3))              // 1
Number(new Boolean(0))              // 0


// Number or new Number()
// 直接返回传入的值


// null和undefined
Number(null)                        // 0
Number(undefined)                   // NaN


// String or new String()
// 只含数字的字符串返回对应数值，前导0将被忽略，浮点只能出现一次
Number('1')                         // 1
Number('-1')                        // -1
Number('01')                        // 1
Number('1.1')                       // 1.1
Number('-01.1')                     // -1.1

// 有效十六进制格式的字符串返回对应十进制数值
Number('0xf')                       // 15

// 空字符串，空格和换行符返回0
Number('')                          // 0
Number(' ')                         // 0
Number('\n')                        // 0

// 'Infinity'和'-Infinity'
Number('Infinity')                  // Infinity
Number('-Infinity')                 // -Infinity

// 以上情况之外的字符串一律返回NaN
Number('a')
Number('1a')
Number('a1')
Number('1.1.1')
Number('NaN')                       // 一律返回NaN


// Object
// 对于对象类型，根据ECMAScript规范，会将对象的[[Primitive Value]]转为Number类型
// 而Number()取得[[Primitive Value]]的方式是先调用valueOf() 如果result还是对象则继续调用toString()
// 于是就有很多有意思的结果返回

Number({})                          // NaN
Number({a: 1})                      // NaN
Number(() => 1)                     // NaN
// 以上传入的对象类型先调用valueOf方法返回传入对象的字面量，然后继续调用toString方法
// 对象字面量返回'[object Object]'，函数字面量返回函数体字符串'() => 1'
// 然后按字符串的规则转换，这些值都是NaN
// 于是如果对以上三个对象调用全局的isNaN方法将会返回true

Number(new Boolean(0))              // 0
Number(new String(0))               // 0
Number(new Number(0))               // 0
Number([])                          // 0
Number([1.1])                       // 1.1
Number(['1'])                       // 1
Number([1, 2])                      // NaN
// 以上前三个都是new调用内置基本类型构造函数得到的包装对象，虽然前面注释的返回结果和直接传入对应类型的值一样
// 但其实JS内部首先对传入的对象调用了valueOf方法，此时返回的[[Primitive Value]]就是对应类型的值，于是按照前述转换规则进行转换
// 数组也是对象类型，所以也会根据它的[[Primitive Value]]来决定转换的结果，JS内部首先对它调用valueOf方法，返回的依然是数组的字面量
// 所以继续调用toString()方法
// 数组的toString()方法会对数组的每一个元素调用toString()，然后返回一个拼接在一起的字符串，数组中的`,`也会转为字符串
// 对于空数组则会返回空字符串
// 所以上面的数组的转换可以依次看作: Number(''), Number('1.1'), Number('1'), Number('1,2') 然后按字符串的规则进行转换
```

### parseInt()和parseFloat()

这两个方法主要是针对字符串进行转换

ES6以后，在Number对象上也增加了这两个方法`Number.parseInt()`和`Number.parseFloat()`，和全局对象上的方法实际上是同一个函数

```js
Number.parseInt === parseInt        // true
Number.parseFloat === parseFloat    // true
```

parseInt接受两个参数，第一个是要转换的值，第二个是转换基数(2到36的整数)，通常第二个参数默认都会传入10表示按十进制转换

parseInt会将传入的目标值转换为整数，通常都是字符串，如果传入的值不是字符串，则会对该值调用toString方法

然后从字符串的第一个字符开始依次解析，开头和结尾的空格符将被忽略，每个有效的数字字符都会转换为数字，直到遇到第一个非数字字符，此时截取前面的结果返回

如果首字符就不能转为数字，那么将直接返回NaN

如果不传第二个参数，则结果有可能会与预期不符，比如传入的值开头包含了合法的十六进制字符串

```js
parseInt('01a')         // 1
parseInt(' 0.1')        // 0
parseInt('a01')         // NaN
parseInt([])            // NaN
parseInt([1.1])         // 1
parseInt('0xA')         // 10
parseInt('0xA', 10)     // 0 
```

对于Infinity和-Infinity

```js
parseInt(Infinity)          // NaN
parseInt(-Infinity)         // NaN
```

parseInt会将传入的目标值转换为浮点数，但如果解析的结果是整数，那么也会返回整数

parseFloat只接受一个参数，即要转换的字符串，整体处理方式基本上和parseInt相同，只是会额外解析第一个`.`浮点字符

parseFloat没有转换基数的参数，对于合法的十六进制字符串永远返回0

parseFloat可以识别科学计数法表示的字符串而parseInt不能

parseFloat可以正确返回Infinity和-Infinity

```js
parseFloat('2.00')          // 2
parseFloat(' 12.3.1')       // 12.3
parseFloat('1.1a')          // 1.1
parseFloat('a1.1')          // NaN
parseFloat([])              // NaN
parseFloat([1.1])           // 1.1
parseFloat('0xA')           // 0

parseInt('1e2')             // 1
parseFloat('1e2')           // 100
parseFloat(Infinity)        // Infinity
parseFloat(-Infinity)       // -Infinity
```

### 一元加操作符的黑科技

`+`或`-`操作符放在非数字类型的值前面时，会对该值像调用`Number()`方法一样首先进行类型转换，所以有的人为了简便，常常会在数字字符串转Number类型时用在前面写上一元加操作符

```js
var str = '1.1'
+a // 1.1
```

但其实前面已经可以看到`Number()`在转换时，得到的返回值情况比较复杂，因此个人认为在制定代码规范时，应该要求String转换为Number类型时应该根据实际需求明确使用`parseInt()`或者`parseFloat()`，禁止使用`Number()`，更禁止使用`+'1.1'`这样的方式来进行转换


参考资料
- 《JavaScript高级程序设计》（第三版）
- 《ES6标准入门》（第三版）
- 《JavaScript语言精粹》
-  [MDN文档 - Number对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number)
-  [MDN文档 - parseInt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
-  [MDN文档 - parseFloat](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseFloat)
