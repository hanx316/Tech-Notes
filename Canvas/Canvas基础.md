# Canvas基础

关于Canvas的基础教程，网上已经有很多了，这里特别推荐两个学习材料：

- MDN上的[Canvas教程](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial)

- 大漠老师网站关于[Canvas的文章集](https://www.w3cplus.com/blog/tags/604.html)

本文主要是一些基础内容的笔记


## canvas元素

### 检测canvas元素

支持canvas元素的浏览器会忽略canvas标签内的内容，因此在做向下兼容的时候可以将替代canvas显示的内容放到canvas标签内

检查是否支持canvas可以通过选中该元素dom对象，检查其上是否具有`getContext`方法

```js
var canvas = document.querySelector('#canvas')
if (!canvas.getContext) {
  alert('Your Browser does not support canvas.')
  return
}
```


## 坐标系

canvas或者说浏览器中，都以左上角为原点，向右形成x轴，向下形成y轴，整个屏幕区域(canvas画布区域)就是第一象限

区域内每一个像素点都有x,y坐标

3d情况下还有一条z轴，以x轴和y轴的交点为原点，两轴形成的平面垂直方向移动，距离人越近，z轴值越大，距离人越远，z轴值越小


### canvas元素的宽高

canvas元素在未设置宽高属性时默认宽300px，高150px

可以通过css来设置canvas的宽高，但如果css设置属性值与上面的默认值不成比例的话，图像会扭曲，所以通常canvas的宽高都用元素的属性去设置


## 绘制路径

beginPath方法会清空之前绘制的信息，开始一条新的路径，在每次绘制之前调用

lineWidth方法设置线段宽度

strokeStyle方法设置颜色

```js
ctx.beginPath()
ctx.lineWidth = 1
ctx.strokeStyle = 'tomato'
```

### 线段

两点构成一条线段，moveTo方法选择起始点，lineTo方法表示终点，可以连续调用，最后调用stroke方法连点成线

```js
ctx.moveTo(x, y)
ctx.lineTo(x, y)
ctx.lineTo(x, y)
ctx.stroke()
```

如果在调用stroke方法之前调用closePath方法，会让起点和终点相连，绘制出一个封闭的图形

可以使用createLinearGradient方法绘制渐变的线段

```js
ctx.beginPath()
var canvasGradient = ctx.createLinearGradient(10, 200, 110, 300)
canvasGradient.addColorStop(0, 'red')
canvasGradient.addColorStop(0.2, 'orange')
canvasGradient.addColorStop(0.4, 'yellow')
canvasGradient.addColorStop(0.6, 'green')
canvasGradient.addColorStop(0.8, 'blue')
canvasGradient.addColorStop(1, 'purple')
ctx.strokeStyle = canvasGradient
ctx.lineWidth = 10
ctx.moveTo(10, 200)
ctx.lineTo(110, 300)
ctx.stroke()
```

由于像素边界的原因，设置的`lineWidth = 1`，如果设置的左边点是整数值，得到的线段往往不会是真实的一像素

>>>如果在像素边界处绘制一条1像素宽的垂直线段，那么Canvas的绘图环境对象会试着将半个像素画在边界中线的右边，将另外半个像素画在边界中线的左边。然而，在一个整像素的范围内绘制半个像素宽的线段是不可能的，所以左右两个方向上的半像素都被扩展为1个像素。

因此，要得到真实的1像素线段，需要在两个像素之间进行绘制，取0.5像素的坐标值

```js
// 下面两条线段可以明显对比出差异
ctx.lineWidth = 1
ctx.strokeStyle = '#000'
ctx.beginPath()
ctx.moveTo(10, 150)
ctx.lineTo(100, 150)
ctx.stroke()

ctx.beginPath()
ctx.moveTo(10.5, 160.5)
ctx.lineTo(100.5, 160.5)
ctx.stroke()
```

===

以下还没写

### 填充

## 绘制圆弧和圆