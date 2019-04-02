# 解构赋值的使用姿势

解构赋值 (Destructuring assignment) 是一种写起来很 cool 但是理解起来可能比较反直觉的赋值手段。

个人是这么来理解解构的形式的，赋值符号右边是某种结构的数据（个人称为数据结构），赋值符号左边是与右侧结构相同的模式（称为模式结构），在模式结构中值的位置就是最终要从数据结构中提取对应值的赋值目标。

### JSON 数据提取赋值

最常用的姿势之一

```js
let data = { name: 'test', age: 26, hobbies: ['read book', 'listen to music'] }
let { name, hobbies: [firstHobby] } = data
```

### 函数返回值提取赋值

其实就是提取 JSON 数据换个思路

```js
function foo() {
  return {
    foo: 'foo',
    bar: 'bar',
  }
}
let { foo, bar } = foo()
```

### 提取对象的部分属性赋值到另一个对象上

全量属性的同步通常会采用合并对象的方式，如果只需要对象的部分属性，往往会一句一句赋值，其实可以通过解构来实现

```js
let data = {
  name: 'test',
  age: 26,
  hobbies: ['read book', 'listen to music'],
}
let target = {
  name: 'test',
}
;({ age: target.age, hobbies: target.hobbies } = data);
```

### 交换变量

最常用的姿势之一

```js
let [a, b] = [1, 2]
;[a, b] = [b, a]
```

### 函数对象参数提取和默认值

在参数声明的地方对参数进行变量解构可以避免在函数内部一遍遍取值

而默认值在声明处定义也更加合理

```js
function Person({ name = '无名氏', age = 1 } = {}) {
  this.name = name
  this.age = age
}
let option = { name: 'test', age: 26 }
new Person(option)
```

### 对象模块一目了然的引用

如今模块化开发的常用姿势了

```js
import { some_method } from some_module
const { someMethod } = require('some-module')
```

### 嵌套赋值时的注意事项

在嵌套赋值时需要考虑模式中父结构不存在的问题，因为不像扁平结构可以直接为变量赋值 undefined, 如果找不到匹配的值又要在它的基础上取子结构赋值，会直接报错

```js
const data = { foo: 'foo' }
let { no: bar }
```
