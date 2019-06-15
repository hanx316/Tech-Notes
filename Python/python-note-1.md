# Python 学习笔记 - 1

学习 Python 的笔记，会随着对这门语言熟悉程度的提升而补充内容。

## 变量和基础数据类型

### 变量

Python 中没有声明变量的关键字，可以直接由标识符进行声明和赋值。

换言之，可以认为 Python 中使用变量必须进行初始化操作，如果直接输入一个标识符而不赋值，会报错。

```py
a = 1
a + a # 2
x # NameError: name 'x' is not defined
```

Python 使用 `#` 进行单行注释。

### 基础数据类型

在学习一门编程语言的时候，通常会对语言支持的数据类型进行诸如基础类型，引用类型等的划分。

这里提到的基础类型都是 Python 标准库中内置类型的一部分，参考 [标准库-内置类型](https://docs.python.org/zh-cn/3/library/stdtypes.html#built-in-types)， Python 官方应该没有做所谓基础类型这样的划分，都统一称为内置类型，所有的数据类型都是对象。

这里所指的基础数据类型包括：整数，浮点数，字符串，布尔值。

#### 整数和浮点数以及基本运算

整数和浮点数属于三种数字类型（int, float, complex）

参考 [Python 教程-数字](https://docs.python.org/zh-cn/3/tutorial/introduction.html#numbers)

##### 字面值

- [Python 语言参考-整型数字面值](https://docs.python.org/zh-cn/3/reference/lexical_analysis.html#integer-literals)

- [Python 语言参考-浮点数字面值](https://docs.python.org/zh-cn/3/reference/lexical_analysis.html#floating-point-literals)

##### 基本运算

基本运算都和其他语言大同小异，参考 [标准库-数字类型](https://docs.python.org/zh-cn/3/library/stdtypes.html#numeric-types-int-float-complex)

其中有些运算需要特别注意：

```py
1 / 2 # 0.5
1 // 2 # 0
1.0 // 2 # 0.0
-1 / 2 # -0.5
-1 // 2 # -1 说明是负向取整
0.1 + 0.2 # 0.30000000000000004  浮点数运算仍然有精度问题
```

##### 数字类型转换

`int()` 函数可以将任意字符串类型或者数字类型转为整数，使用参考 [标准库内置函数-int](https://docs.python.org/zh-cn/3/library/functions.html#int)

`float()` 函数可以将任意字符串类型或者数字类型转为浮点数，使用参考 [标准库内置函数-float](https://docs.python.org/zh-cn/3/library/functions.html#float)

下面是一些实验：

```py
# int()
int() # 0
int('') # ValueError: invalid literal for int() with base 10: ''
int('a') # ValueError: invalid literal for int() with base 10: 'a'
int('1a') # ValueError: invalid literal for int() with base 10: '1a'
int('00') # 0
int('0100') # 100
int(00) # 0
int(001) # SyntaxError: invalid token
int(001.) # 1
int(001.1) # 1
int(0.) # 0
int(0.5) # 0
int('0.5') # ValueError: invalid literal for int() with base 10: '0.5'
int(-1) # -1
int('-1') # -1
int('-1.1') # ValueError: invalid literal for int() with base 10: '-1.1'
int(0x1) # 1
int('0x1') # ValueError: invalid literal for int() with base 10: '0x1'
int('0x1', 16) # 1
int(True) # 1
int(False) # 0

# float()
float() # 0.0
float(1) # 1.0
float('1') # 1.0
float('a') # ValueError: could not convert string to float: 'a'
float('1.1a') # ValueError: could not convert string to float: '1.1a'
# 大小写不限
float('nan') # nan
float('inf') # inf
float('infinity') # inf
float('') # ValueError: could not convert string to float:
float(0.) # 0.0
float(00) # 0.0
float('00') # 0.0
float(001) # SyntaxError: invalid token
float('001') # 1.0
float(001.) # 1.0
float(001.1) # 1.1
float(-1) # -1.0
float('-1') # -1.0
float('-1.1') # -1.1
float(0x1) # 1.0
float('0x1') # ValueError: could not convert string to float: '0x1'
float(True) # 1.0
float(False) # 0.0
```

总结就是使用 `int()` 函数时最好确保传参是浮点数，或者整数型的字符串；使用 `float()` 函数时如果传参不是数字类型，最好确保是数字型字符串；二者都可以传入布尔值。

#### 字符串以及基本操作

字符串在 Python 中是 str 对象，又叫文本序列类型，属于序列类型的一种。

参考 [Python 教程-字符串](https://docs.python.org/zh-cn/3/tutorial/introduction.html#strings)

参考 [标准库-文本序列类型](https://docs.python.org/zh-cn/3/library/stdtypes.html#text-sequence-type-str)

使用 `print()` 函数来输出字符串进行实验。

##### 字面值

[Python 语言参考-整型数字面值](https://docs.python.org/zh-cn/3/reference/lexical_analysis.html#strings)

##### 原始字符串

> 字符串和字节串字面值都可以带有前缀 'r' 或 'R'；这种字符串被称为 原始字符串 其中的反斜杠会被当作其本身的字面字符来处理。

```py
print('Let\'s go!')
print(r'Let\'s go!')
```

##### 字节串和编码

Python 字符串使用 Unicode 编码，默认转换格式是 UTF-8。

> 字节串字面值总是带有前缀 'b' 或 'B'；它们生成 bytes 类型而非 str 类型的实例。它们只能包含 ASCII 字符；字节对应数值在128及以上必须以转义形式来表示。

```py
'abc'.encode('utf-8') # b'abc'
'abc'.encode('ascii') # b'abc'
'abc'.encode('utf-32') # b'\xff\xfe\x00\x00a\x00\x00\x00b\x00\x00\x00c\x00\x00\x00'
'钱'.encode('utf-8') # b'\xe9\x92\xb1'
'钱'.encode('utf-32') # b'\xff\xfe\x00\x00\xb1\x94\x00\x00'
```

返回的应该是内存和磁盘中实际存储的编码字节，通过字节串展示出来。

##### 字符串类型转换

`str()` 函数可以将任意合法的对象值返回它的字符串值，参考 [标准库内置函数-str](https://docs.python.org/zh-cn/3/library/functions.html#func-str)

##### 字符串基本操作

由于字符串也是一种序列，所以序列的标准操作（索引、切片、乘法、成员资格检查、长度、最小值和最大值）都适用于字符串，除了为字符串的成员进行赋值。

下面是一些常规操作：

1. 成员访问和查找

```py
# 索引访问
surnames = '赵钱孙李，周吴郑王'
surnames[0] # '赵'
surnames[-1] # '王'

# 切片访问
surnames[2:1] # ''
surnames[2:3] # '孙'
surnames[2:] # '孙李，周吴郑王'

# 查找子串
surnames.find('李') # 3
surnames.find('a') # -1
```

2. 使用 `+` 进行字符串拼接

```py
world = 'world'
print('Hello, ' + world + '!')
```

3. 使用转换说明符组装变量

```py
print('Hello, %s' % 'world')
# 通常配合元组使用
names = ('Jim', 'Lily', 'Lucy')
print('Hello, %s %s and %s' % names)
```

参考 [rintf 风格的字符串格式化](https://docs.python.org/zh-cn/3/library/stdtypes.html#old-string-formatting)

4. 格式字符串语法

```py
'{}, {} and {}'.format('first', 'second', 'third')
'{0}, {1}'.format('hello', 'world')
'{greeting}, {name}'.format(name='Jim', greeting='Hello')
```

参考 [格式字符串语法](https://docs.python.org/zh-cn/3/library/string.html#formatstrings)

参考 [str.format](https://docs.python.org/zh-cn/3/library/stdtypes.html#str.format)

#### 布尔值

布尔值包括两个常量 `True` 和 `False`。

标准库中数字类型有提到，布尔值属于整数的子类型。所以在做数字类型转换的时候可以将 `True` 和 `False` 传给 `int()` 或者 `float()`。

`bool()` 函数可以将任意对象通过逻辑值检测转换为布尔值。

参考 [逻辑值检测](https://docs.python.org/zh-cn/3/library/stdtypes.html#truth)

参考 [标准库内置函数-bool](https://docs.python.org/zh-cn/3/library/functions.html#bool)
