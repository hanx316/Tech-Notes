# 使用 JS-XLXS 读取 Excel 文件

最近需要做一个数据转换为图片的生成工具，显然我们不太可能要求需求方提供 json 结构的数据源， Excel 应该是一种比较普遍的方式。

调研了一些 JS 处理 Excel 文件的开源库， [JS-XLXS](https://github.com/SheetJS/js-xlsx) 看上去还算一个不错的方案。并且我们会约定好 Excel 的结构，所以实现起来也并不复杂。

就这个需求而言，只是一个纯前端的小工具，所以先仅仅介绍在浏览器上的处理方法，直接通过 script 引入。

主要是读取文件的处理，用上传控件提交后，通过 FileReader 读取为二进制字符串，再由 XLSX 提供的二进制读取方式就可以拿到文件的数据。

假定第一行的每一列是一个字段，之后每一行是一个对象的话（类似 MySQL 表结构），可以直接调用 XLSX 提供的 `utils.sheet_to_json` 工具方法。

```js
var input = document.querySelector('#input')
input.addEventListener('change', e => {
  var reader = new FileReader()
  var file = e.target.files[0]
  reader.onload = e => {
    var data = e.target.result
    var workbook = XLSX.read(data, { type: 'binary' })
    var worksheet = XLSX.utils.sheet_to_json(workbook.Sheets.Sheet1)
    console.log(worksheet)
  }
  reader.readAsBinaryString(file)
})
```

这样只要拿到对象化之后的表格数据，剩下的无非就是 json 结构转换了。

参考文章：

[Node读写Excel文件探究实践](https://aotu.io/notes/2016/04/07/node-excel/index.html)

[在 Node.js 中利用 js-xlsx 处理 Excel 文件](https://scarletsky.github.io/2016/01/30/nodejs-process-excel/)

未完待续...



