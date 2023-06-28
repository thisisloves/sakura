---
title: CSS
date: 2018-2-7
tags: CSS
author: 映雪
---

欲守在渡口与吾隔水望，有水花落在肩头和发上，而我再看不见你今生模样，要如何与你共赏这风光。

<!--more-->

### CSS

- CSS 指层叠样式表 (Cascading Style Sheets), CSS 可以同时控制多重网页的样式和布局。
- 多个样式表叠加样式，后书写的样式覆盖前面书写的样式。

### CSS 组成

![截屏2022-05-31 下午5.59.44.png](/images/2022/05/31/lREBNDMbGeoPYa5.png)

- 选择器通常是需要改变样式的 HTML 元素。
- 每条声明由一个属性和一个值组成。
- 属性（property）是设置的样式属性（style attribute）。每个属性有一个值。属性和值被冒号分开。

### CSS 使用

```css
p {
  color: red;
  text-align: center;
}
```

### CSS 注释

- 注释是用来解释你的代码，并且可以随意编辑它，浏览器会忽略它。

```css
/*这是个注释*/
p {
  text-align: center;
  /*这是另一个注释*/
  color: black;
  font-family: arial;
}
```
