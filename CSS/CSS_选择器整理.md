# CSS 选择器整理

关于 CSS 的选择器，[MDN 上这份文档](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors)有详细的介绍。这里做一个核心内容的简单梳理。

## 选择器分类

最基本的选择器

- 通用选择器
- 类型/元素/标签 选择器 (Type Selector)
- id 选择器
- 类选择器
- 分组选择器 (',')

高级一些的选择器

- 属性选择器
- 后代选择器 (Descendant Combinator ' ')
- 子选择器 (Child Combinator '>')
- 一般同胞选择器 (General Sibling Combinator '~')
- 相邻同胞选择器 (Adjacent Sibling Combinator '+')
- 列选择器 (Column Combinator '||')
- 伪类
- 伪元素

## 选择器使用

主要挑一些高级的选择器进行说明。

#### 属性选择器

```
# 匹配具有该属性的
selector[attr]

# 匹配具有该属性，且值等于 value 的
selector[attr=value]

# 匹配具有该属性，且值是空格间隔开，其中包含 value 的，比如匹配 class 包含某个值
selector[attr~=value]

# 匹配具有该属性，且值以 value 开头，类似正则
selector[attr^=value]

# 匹配具有该属性，且值以 value 结尾，类似正则
selector[attr$=value]

# 匹配具有该属性，且值包含 value 作为子串，这个和 ~= 有所覆盖
selector[attr*=value]

# 匹配具有该属性，且值等于 value 或者以 value- 开头，比如用作语言匹配 lang|=zh => zh-CN / zh-TW
selector[attr|=value]
```

#### Combinators

Combinator 翻译成组合子，是指 CSS 选择器中的几种配合选择器使用的选择符号，目前包括五个：' ', '>', '~', '+', '||'。

列选择器 ('||') 目前支持度不高，且限于 table 元素，暂且不管。

后代选择器 (' ') 匹配所有后代（子级，孙级...），比如 p span。

子选择器 ('>') 匹配直接后代（子级），比如 p > span。

一般同胞选择器 ('~') 匹配前者元素之后的所有后者元素，且所有这些元素都拥有共同的父元素，比如 p ~ img。

相邻同胞选择器 ('+') 匹配前者元素之后紧随的后者元素，且所有这些元素都拥有共同的副元素，比如 p + p。

#### 伪类

常见的比如：

- :link
- :visited
- :hover
- :focus
- :active
- :target / :target:not(selector)

值得注意的是，link 和 visited 是一组，hover 和 focus 也是一组，最后是 active 这三组的顺序不能进行改变，link 和 visit 是固定的状态，hover, focus, active 都是用户的交互行为发生时的样式，如果交互的伪类放在前面会被 link/visit 的样式覆盖从而无法生效。

active 发生在点击时，一定发生在 hover 或者 focus 之后，如果 active 的样式放在 hover/focus 之前，那么也将被覆盖而无法生效。

还有一些比较有用的结构化伪类：

- first-child
- last-child
- nth-child
- first-of-type
- last-of-type
- nth-of-type
- nth-last-child
- nth-last-of-type

