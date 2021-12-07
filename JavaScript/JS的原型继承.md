# JS 的原型继承

我们说 JS 是一门基于对象的语言，在 JS 中也实现了对象的继承。所谓继承，简单理解就是一个对象可以访问另一个对象的属性和方法。

最典型的继承实现方式有两种，基于类的设计和基于原型的设计。C++、Java、PHP、c# 等面向对象语言都采用了前者，这是一种非常经典的继承设计模式，提供了丰富的继承规则，能够很好地抽象和描述需要面对的场景，但是面对复杂的业务场景时，往往也会带来更高的设计和代码的复杂度。

基于原型的继承设计则简单和轻量许多，每个对象都拥有一个原型对象。对象除了可以访问自身的属性和方法外，还可以访问它的原型对象上的属性和方法。原型对象作为对象自然也可以继续拥有原型对象，直到一个根对象，它的原型对象是 `null`。这就形成了一条所谓的原型链，那么当访问一个对象的属性和方法时，首先在对象本身上查找，找不到时再在它的原型对象上查找，找到则返回，找不到则如此循环往复直到根对象。

在 JS 中，原型继承则还有几个特殊的点需要注意：

1. 由于 JS 中的函数是一等公民，也是对象，所以函数当然也拥有原型对象
2. JS 可以使用 `new` 操作符来创建对象，创建出的实例对象的原型对象是构造函数的 `prototype` 属性
3. 函数的原型对象并不是函数的 `prototype` 属性，就函数本身而言，二者没有关系
4. 在 V8 中，对象的原型对象通过对象的 `__proto__` 属性关联，但它是引擎内置的属性，在开发中使用是不安全且影响性能的
5. JS 提供了一组 API 来访问和修改对象的原型对象：`Object.getPrototypeOf()` 和 `Object.setPrototypeOf()`
6. JS 中对象原型链的根对象是 `Object.prototype`

下面，直接从代码来看看 JS 的原型继承和原型链。

```js
var animal = {
  type: "Default",
  color: "Default",
  getInfo: function () {
    return `Type is: ${this.type}，color is ${this.color}.`
  },
}
var dog = {
  type: "Dog",
  color: "Black",
}
// 将两个对象以原型继承方式关联起来
Object.setPrototypeOf(dog, animal)
console.log(dog.getInfo())

// 构造函数继承
console.log('=========')
function AnimalFactory(type, color) {
  this.type = type
  this.color = color
}
console.log('AnimalFactory.prototype.constructor', AnimalFactory.prototype.constructor === AnimalFactory, AnimalFactory.prototype.constructor === Object)
AnimalFactory.prototype = animal
console.log('AnimalFactory.prototype.constructor 变化', AnimalFactory.prototype.constructor === Object)
// // fix
// AnimalFactory.prototype.constructor = AnimalFactory

const cat = new AnimalFactory('cat', 'yellow')
console.log(cat.getInfo())
console.log(cat instanceof AnimalFactory, dog instanceof AnimalFactory)
console.log(cat.__proto__ === dog.__proto__)
const bird = new AnimalFactory('bird', 'blue')
console.log(bird.__proto__ === cat.__proto__, bird.__proto__ === dog.__proto__)


// 一条原型链
console.log('=====从实例开始的一条原型链=====')
console.log(cat.__proto__ === AnimalFactory.prototype)
console.log(AnimalFactory.prototype.__proto__ === Object.prototype)
console.log(Object.prototype.__proto__ === null)
console.log('=====从实例开始分析 constructor =====')
console.log('cat.constructor === AnimalFactory.prototype.constructor', cat.constructor === AnimalFactory.prototype.constructor)
console.log('cat.__proto__.constructor === cat.constructor', cat.__proto__.constructor === cat.constructor)
// 分析
console.log('分析')
console.log('cat.constructor === Object', cat.constructor === Object)
console.log('AnimalFactory.prototype.constructor === Object', AnimalFactory.prototype.constructor === Object)
console.log('Because AnimalFactory.prototype.constructor === animal.constructor', AnimalFactory.prototype.constructor === animal.constructor)
// fix
console.log('fix')
AnimalFactory.prototype.constructor = AnimalFactory
console.log('after fix AnimalFactory.prototype.constructor === AnimalFactory', AnimalFactory.prototype.constructor === AnimalFactory)
console.log('cat.constructor === AnimalFactory', cat.constructor === AnimalFactory)
console.log('Function.prototype constructor', Function.prototype.constructor === Function)
console.log('Object.prototype constructor', Object.prototype.constructor === Object)
// 再看结论
console.log('结论')
console.log('cat.constructor === AnimalFactory.prototype.constructor', cat.constructor === AnimalFactory.prototype.constructor)
console.log('cat.__proto__.constructor === cat.constructor', cat.__proto__.constructor === cat.constructor)


// 一条原型链
console.log('=====从构造函数开始的一条原型链=====')
console.log(AnimalFactory.__proto__ === Function.prototype)
console.log(Function.prototype.__proto__ === Object.prototype)
console.log(Object.prototype.__proto__ === null)

// 一条原型链
console.log('=====从 Function() 开始的一条原型链=====')
console.log(Function.__proto__ === Function.prototype)
console.log(Function.prototype.__proto__ === Object.prototype)
console.log(Object.prototype.__proto__ === null)

// 一条原型链
console.log('=====从 Object() 开始的一条原型链=====')
console.log(Object.__proto__ === Function.prototype)
console.log(Function.prototype.__proto__ === Object.prototype)
console.log(Object.prototype.__proto__ === null)
```

## 模拟 new 实现

```js
function newFn(constr) {
  if (typeof constr !== 'function') {
    throw new Error('constructor must be a function')
  }
  const obj = Object.create(constr.prototype)
  // 等价于
  // const obj = Object.create(null)
  // Object.setPrototypeOf(obj, constr.prototype)
  const args = [].slice.call(arguments, 1)
  const ret = constr.apply(obj, args)
  if (typeof ret === 'object' || typeof ret === 'function' && ret !== null) {
    return ret
  }
  return obj
}

function AnimalFactory(type, color) {
  this.type = type
  this.color = color
}
AnimalFactory.prototype.getInfo = function () {
  return `Type is: ${this.type}，color is ${this.color}.`
}

const cat = newFn(AnimalFactory, 'cat', 'yellow')
console.log(cat, cat.getInfo())
console.log(cat.constructor)
console.log('cat.constructor === AnimalFactory.prototype.constructor', cat.constructor === AnimalFactory.prototype.constructor)
console.log('cat.__proto__.constructor === cat.constructor', cat.__proto__.constructor === cat.constructor)
```

## 模拟 instanceOf 实现，原型链查找

```js
function instance_of(obj, constr) {
  if (typeof obj !== 'object' && typeof obj !== 'function' || obj === null) {
    return false
  }
  let proto = Object.getPrototypeOf(obj)
  while (true) {
    if (proto === null) return false
    if (proto === constr.prototype) return true
    proto = Object.getPrototypeOf(proto)
  }
}
console.log('instance_of')
console.log( instance_of(cat, AnimalFactory) )
console.log( instance_of({}, Object) )
console.log( instance_of(() => {}, Function) )
console.log( instance_of([], Array) )
console.log( instance_of([], Object) )
```
