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

===

以下还没写

## 绘制路径

### 线段

### 填充

## 绘制圆弧和圆