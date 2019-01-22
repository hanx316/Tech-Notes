# D3绘制的SVG图像转为图片保存

假定我们已经使用 D3 绘制了图像，现在需要将它转换为 png 或者 jpg 格式的图片保存起来。

```js
// d3 画布大小
let width = 400
let height = 400

var html = d3.select('svg')
  .attr('version', 1.1)
  .attr('xmlns', 'http://www.w3.org/2000/svg')
  .node().parentNode.innerHTML
html = html.replace(/<!--[\w\W]*?-->/, '').trim()

var imgsrc = `data:image/svg+xml;base64, ${btoa(html)}`
var image = new Image()
image.src = imgsrc
image.onload = function() {
  var canvas = document.createElement('canvas')
  var context = canvas.getContext('2d')
  canvas.width = width
  canvas.height = height
  context.drawImage(image, 0, 0, 400, 400, 0, 0, 400, 400)
  
  var canvasdata = canvas.toDataURL('image/jpg')
  var a = document.createElement('a')
  a.download = 'sample.jpg'
  a.href = canvasdata
  a.click()
}
```

核心逻辑：

- 将 svg 标签通过 `btoa` 方法编码为 base64 字符串创建图片

- 将图片通过 canvas 画出来

- 再将 canvas 转为需要的图片格式的 DataURL

- 通过 a 标签完成最后一步

参考：

[将SVG保存为图片[译]](https://www.tangshuang.net/3595.html)，基本上就是按这篇译文的套路来的。
