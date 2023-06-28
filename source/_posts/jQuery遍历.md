---
title: jQuery遍历
date: 2018-9-26
tags: jQuery
author: 映雪
---

人去空流水，花飞半掩门。乱山何处觅行云。又是一钩新月照黄昏。

<!--more-->

### JQuery 遍历

- jQuery 遍历，意为"移动"，用于根据其相对于其他元素的关系来"查找"（或选取）HTML 元素。以某项选择开始，并沿着这个选择移动，直到抵达您期望的元素为止。

![截屏2022-05-30 下午2.32.34.png](/images/2022/05/30/2PceMEYqpuNrfxh.png)

1. `<div>` 元素是 `<ul>` 的父元素，同时是其中所有内容的祖先。
2. `<ul>` 元素是 `<li>` 元素的父元素，同时是 `<div>` 的子元素
3. 左边的 `<li>` 元素是 `<span>` 的父元素，`<ul>`的子元素，同时是 `<div>` 的后代。
4. `<span>` 元素是 `<li>` 的子元素，同时是 `<ul>` 和 `<div>` 的后代。
5. 两个 <li> 元素是同胞（拥有相同的父元素）。
6. 右边的 `<li>` 元素是 `<b>` 的父元素，`<ul>` 的子元素，同时是 `<div>` 的后代。
7. `<b>` 元素是右边的 `<li>` 的子元素，同时是 `<ul>` 和 `<div>` 的后代。

- 通过 jQuery 遍历，您能够从被选（当前的）元素开始，轻松地在家族树中向上移动（祖先），向下移动（子孙），水平移动（同胞）。这种移动被称为对 DOM 进行遍历。


### JQuery 遍历祖先

- 向上遍历 DOM 树

1. parent()  返回被选元素的直接父元素。
2. parents() 返回被选元素的所有祖先元素，它一路向上直到文档的根元素 (<html>)。
3. parentsUntil() 返回介于两个给定元素之间的所有祖先元素。

```js
$(document).ready(function(){
  $("span").parent();
});
$(document).ready(function(){
  $("span").parents();
});
$(document).ready(function(){
  $("span").parentsUntil("div");
});
```

### JQuery 遍历后代

- 向下遍历 DOM 树

1. children() 返回被选元素的所有直接子元素。
2. find()     返回被选元素的后代元素，一路向下直到最后一个后代。


```js
$(document).ready(function(){
  $("div").children();
});
$(document).ready(function(){
  $("div").find("span");
});
```

### JQuery 遍历同胞

- 在 DOM 树中遍历元素的同胞元素。

1. siblings()   返回被选元素的所有同胞元素。
2. next()       返回被选元素的下一个同胞元素。
3. nextAll()    返回被选元素的所有跟随的同胞元素。
4. nextUntil()  返回介于两个给定参数之间的所有跟随的同胞元素。
5. prev()       返回的是前面的同胞元素
6. prevAll()    返回的是前面的同胞元素
7. prevUntil()  返回的是前面的同胞元素


```js
$(document).ready(function(){
  $("h2").siblings();
});
$(document).ready(function(){
  $("h2").next();
});
$(document).ready(function(){
  $("h2").nextAll();
});
$(document).ready(function(){
  $("h2").nextUntil("h6");
});
```

### JQuery 遍历过滤

- 基于其在一组元素中的位置来选择一个特定的元素。

1. first() 返回被选元素的首个元素。
2. last()  返回被选元素的最后一个元素。
3. eq()    返回被选元素中带有指定索引号的元素。
4. filter() 选取匹配某项指定标准的元素
5. not()  选取不匹配某项指定标准的元素

```js
$(document).ready(function(){
  $("div p").first();
});
$(document).ready(function(){
  $("div p").last();
});
$(document).ready(function(){
  $("p").eq(1);
});
$(document).ready(function(){
  $("p").filter(".url");
});
$(document).ready(function(){
  $("p").not(".url");
});
```