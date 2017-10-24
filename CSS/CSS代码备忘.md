# CSS代码备忘

主要是记录平时开发中处理的一些常见但又不常写的样式需求

---

### 禁止文本选择

```css
user-select: none;
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

备忘。。。