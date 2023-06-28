---
title: jQuery基础
date: 2018-4-25
tags: jQuery
author: 映雪
---

自君之出矣，不复理残机。思君如满月，夜夜减清辉。

<!--more-->

### jQuery

- jQuery 是一个 JavaScript 库。jQuery 极大地简化了 JavaScript 编程。
- jQuery 兼容于所有主流浏览器, 包括 Internet Explorer 6。

### jQuery 语法

- jQuery 语法是通过选取 HTML 元素，并对选取的元素执行某些操作。

```js
$(selector).action();
```

- 美元符号定义 jQuery
- 选择符（selector）"查询"和"查找" HTML 元素
- jQuery 的 action() 执行对元素的操作

### 文档就绪事件

实例中的所有 jQuery 函数位于一个 document ready 函数中

```js
$(document).ready(function () {
  // 执行代码
});
// 或者
$(function () {
  // 执行代码
});
```

- 为了防止文档在完全加载（就绪）之前运行 jQuery 代码，即在 DOM 加载完成后才可以对 DOM 进行操作。
- 如果在文档没有完全加载之前就运行函数，操作可能失败。

### jQuery 入口函数与 JavaScript 入口函数的区别

jQuery 的入口函数是在 html 所有标签(DOM)都加载之后，就会去执行。
JavaScript 的 window.onload 事件是等到所有内容，包括外部图片之类的文件加载完后，才会执行。

```js
$(document).ready(function () {
  // 执行代码
});
```

- JavaScript 入口函数:

```js
window.onload = function () {
  // 执行代码
};
```
