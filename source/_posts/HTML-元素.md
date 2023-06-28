---
title: HTML元素
date: 2017-2-2
tags: HTML
author: 映雪
---

枕上一书三生誓，殷切与君语迟迟。 佛铃花落醉尘火，执念难绝情成痴。

<!--more-->

### HTML 元素

HTML 文档由 HTML 元素定义。

|         开始标签         |   元素内容   | 结束标签 |
| :----------------------: | :----------: | :------: |
|          `<p>`           | 这是一个段落 |  `</p>`  |
| `<a href="default.htm">` | 这是一个链接 |  `</a>`  |
|          `<br>`          |     换行     |          |

- 开始标签常被称为起始标签（opening tag），结束标签常称为闭合标签（closing tag）。

### HTML 元素语法

- HTML 元素以开始标签起始
- HTML 元素以结束标签终止
- 元素的内容是开始标签与结束标签之间的内容
- 某些 HTML 元素具有空内容（empty content）
- 空元素在开始标签中进行关闭（以开始标签的结束而结束）
- 大多数 HTML 元素可拥有属性

### HTML 元素类型

1. 块状元素

(1)能够识别宽高
(2)margin 和 padding 的上下左右均对其有效
(3)可以自动换行
(4)多个块状元素标签写在一起，默认排列方式为从上至下

|    标签    |                            描述                            |
| :--------: | :--------------------------------------------------------: |
|  address   |                            地址                            |
| blockquote |                           块引用                           |
|   center   |                         举中对齐块                         |
|    dir     |                          目录列表                          |
|    div     |          常用块级容易，也是 css layout 的主要标签          |
|     dl     |                          定义列表                          |
|  fieldset  |                        form 控制组                         |
|    form    |                          交互表单                          |
|     h1     |                           大标题                           |
|     h2     |                           副标题                           |
|     h3     |                          3 级标题                          |
|     h4     |                          4 级标题                          |
|     h5     |                          5 级标题                          |
|     h6     |                          6 级标题                          |
|     hr     |                         水平分隔线                         |
|  isindex   |                        input prompt                        |
|    menu    |                          菜单列表                          |
|  noframes  | frames 可选内容，（对于不支持 frame 的浏览器显示此区块内容 |
|  noscript  |    可选脚本内容（对于不支持 script 的浏览器显示此内容）    |
|     ol     |                          排序表单                          |
|     p      |                            段落                            |
|    pre     |                         格式化文本                         |
|   table    |                            表格                            |
|     ul     |                         非排序列表                         |

2. 内联元素

(1)设置宽高无效
(2)对 margin 仅设置左右方向有效，上下无效；padding 设置上下左右都有效，即会撑大空间，行内元素尺寸由内含的内容决定，盒模型中 padding， border 与块级元素并无差异，都是标准的盒模型，但是 margin 却只有水平方向的值，垂直方向并没有起作用。行内元素的水平方向的 padding-left，padding-right，margin-left，margin-right 都产生边距效果，但是竖直方向的 padding-top，padding-bottom，margin-top， margin-bottom 都不会产生边距效果。padding 设置上下左右都有效，即会撑大空间但是不会产生边距效果。　　　
(3)不会自动进行换行

|   标签   |               描述               |
| :------: | :------------------------------: |
|    a     |               锚点               |
|   abbr   |               缩写               |
| acronym  |               首字               |
|    b     |           粗体(不推荐)           |
|   bdo    |          bidi override           |
|   big    |              大字体              |
|    br    |               换行               |
|   cite   |               引用               |
|   code   | 计算机代码(在引用源码的时候需要) |
|   dfn    |             定义字段             |
|    em    |               强调               |
|   font   |         字体设定(不推荐)         |
|    i     |               斜体               |
|   kbd    |           定义键盘文本           |
|  label   |             表格标签             |
|    q     |              短引用              |
|    s     |          中划线(不推荐)          |
|   samp   |        定义范例计算机代码        |
|  small   |            小字体文本            |
|   span   |   常用内联容器，定义文本内区块   |
|  strike  |              中划线              |
|  strong  |             粗体强调             |
|   sub    |               下标               |
|   sup    |               上标               |
| textarea |          多行文本输入框          |
|    tt    |             电传文本             |
|    u     |              下划线              |

3. 行内块状元素

(1)不自动换行
(2)能够识别宽高
(3)默认排列方式为从左到右

| 标签  |  描述  |
| :---: | :----: |
|  img  |  图片  |
| input | 输入框 |

### 元素类型转换

- HTML 可以将元素分类方式分为行内元素、块状元素和行内块状元素三种。 这三者是可以互相转换的，使用 display 属性能够将三者任意转换

1. display:inline;转换为行内元素
2. display:block;转换为块状元素
3. display:inline-block;转换为行内块状元素

### 列表元素

- 无序列表

```html
<ul>
  <li>无序</li>
  <li>无序</li>
</ul>
```

- 有序列表

```html
<ol>
  <li>有序</li>
  <li>有序</li>
  <li>有序</li>
</ol>
```

- 自定义列表

```html
<dl>
  <dt>标题</dt>
  <dd>内容</dd>
  <dd>内容</dd>
</dl>
```
