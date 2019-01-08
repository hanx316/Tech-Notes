# 如何获取一个元素的宽高

一个基础问题，我想到有三种方式

1. 元素的 offsetWidth 和 offsetHeight 属性，最直观

包括了元素的 content, padding, scrollbar(if rendered), border, margin 的所有宽高

```js
var width = HTMLElement.offsetWidth
var height = HTMLElement.offsetHeight
```

获取到的值是 Number 类型，如果该元素设置了 `display: none` 那么将得到 0

和这一对属性区别的是 clientWidth 和 clientHeight, 它们仅包括元素的 content 和 padding 宽高

2. 通过 getBoundingClientRect 方法计算

```js
var rect = Element.getBoundingClientRect()
var width = rect.right - rect.left
var height = rect.bottom - rect.top
```
getBoundingClientRect 方法返回一个包含 top, left, right, bottom 属性的对象，分别是元素盒子四边距离视口顶部和左侧的距离，值都是 Number 类型

3. 通过计算样式直接获取

```js
var o = window.getComputedStyle(element)
// 对于宽高属性可以直接获取
var width = o.width
var height = o.height
// 调用 getPropertyValue 方法获取更保险
var width = o.getPropertyValue('width')
var height = o.getPropertyValue('height')
```

需要特别说明的是，**获取到的值是像 '100px' 这样的 String 类型，如果要参与计算的话需要 parse 一下**
