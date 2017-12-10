# CSS代码备忘

主要是记录平时开发中处理的一些常见但又不常写的样式需求

### 禁止文本选择

```css
.text {
  user-select: none;
}
```

### webkit内核浏览器滚动条美化(扒有道云笔记)

```css
::-webkit-scrollbar {
  height: 8px;
  width: 8px;
  background: transparent;
  border-radius: 4px;
}
::-webkit-scrollbar-button {
  display: none;
}
::-webkit-scrollbar-track {
  background-color: transparent;
}
::-webkit-scrollbar-track-piece {
  background: transparent;
}
::-webkit-scrollbar-thumb {
  width: 8px;
  min-height: 15px;
  background: #c1c1c1;
  border-radius: 4px;
}
::-webkit-scrollbar-thumb:hover {
  background: #7d7d7d;
}
::-webkit-scrollbar-thumb:active {
  background: #7d7d7d;
}
```

### 多行文本末尾显示省略号

主要是针对webkit内核实现

```css
p {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

跨浏览器兼容方案参考，定位一个伪元素进行模拟

```css
p {
  position: relative;
  height: 4.2em;
  /* 3 times the line-height to show 3 lines */
  line-height: 1.4em;
  overflow: hidden;
}
p::after {
  position: absolute;
  bottom: 0;
  right: 0;
  padding: 0 20px 1px 45px;
  font-weight: bold;
  content: '...';
}
```

另附张大神的一篇旧文参考

[关于文字内容溢出用点点点(…)省略号表示](http://www.zhangxinxu.com/wordpress/2009/09/%E5%85%B3%E4%BA%8E%E6%96%87%E5%AD%97%E5%86%85%E5%AE%B9%E6%BA%A2%E5%87%BA%E7%94%A8%E7%82%B9%E7%82%B9%E7%82%B9-%E7%9C%81%E7%95%A5%E5%8F%B7%E8%A1%A8%E7%A4%BA/)

### 单行文本溢出末尾显示省略号

```css
.text {
  overflow:hidden; 
  text-overflow: ellipsis; 
  white-space: nowrap; 
}
```

### CSS水平翻转和垂直翻转

```css
.reverse {
  /* 水平翻转方案一 */
  transform: scaleX(-1);

  /* 水平翻转方案二 */
  transform: rotateY(180deg);

  /* 垂直翻转替换到相反对应的轴即可 */
  transform: scaleY(-1);

  transform: rotateX(180deg);
}
```