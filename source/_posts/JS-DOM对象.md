---
title: DOM对象
date: 2019-3-29
tags: JS
author: 映雪
---

初闻不知曲中意、再听已是曲中人。

<!--more-->

### DOM (文档对象模型)

- 当网页被加载时，浏览器会创建页面的文档对象模型（Document Object Model）

![截屏2022-03-11 下午4.13.32.png](/images/2022/03/11/LXoMDwVl8JYQK1i.png)

- JavaScript 能够改变页面中的所有 HTML 元素
- JavaScript 能够改变页面中的所有 HTML 属性
- JavaScript 能够改变页面中的所有 CSS 样式
- JavaScript 能够对页面中的所有事件做出反应


### 获取 DOM

1. 元素节点选择器

```js
var obox=document.getElementById("box"); //返回值是一个元素对象
console.log(obox);
```

2. class 选择器

```js
var acont=document.getElementsByClassName("cont"); //无论任何情况，class选择器，返回值都是数组，如果需要操作其中的元素，必须使用索引解析或者遍历
console.log(acont[0]);
console.log(acont[acont.length-1])
```

3. 标签选择器

```js
var aspan=document.getElementsByTagName("span");
console.log(aspan) //无论任何情况，标签选择器的返回值都是数组，如果需要操作其中的元素，必须使用索引解析或者遍历
```

4. 标签选择器

```js
var auser=document.getElementsByName("pass");
console.log("auser");//无论什么时候，标签选择器返回值都是数组，如果需要操作其中的元素，必须使用索引解析或者遍历
```

5. ES5 中新增选择器


```js
var a=document.querySelector(".cont");
console.log(a); //返回的是单个.str 对象，.str 有多个，但是也只能返回一个

var b=document.querySelectorAll("#box");
console.log(b); //返回#box，返回数组，有多少个返回多少个
```

6. 关系选择器

```js
var olist=document.querySelector(".list")

// 父选子元素
var achild=olist.children;
console.log(achild);

// 子选父元素
var oparent=achild[2].parentNode;
console.log(oparent);

// 第一个子元素和最后一个子元素
console.log(olist.fristElementChild)
console.log(olist.lastElementChild)
 
// 兄弟选择器
console.log(olist.previousElementChild)    //previous以前
console.log(olist.nextElementSibing)

```

### 获取/设置 DOM 属性

1. 点语法

```html
<body>
<div class="box" id="box1" title="这是title" alt="这是alt" href="这是href" src="这是src" abc="123" qwe="hahaha" zxc="liyang">
<em>123132</em>
</div>
<a  href="123112">123</a>
</body>
<script>
var obox=document.querySelector(".box");

console.log(obox.className);  // box
obox.className="xixi";
console.log(obox.className);  // xixi

console.log(obox.id);    //box1
obox.id="heheh";
console.log(obox.id);  // heheh
console.log(obox.title);// 这是title
obox.title="aiaiaiia";
console.log(obox.title); //aiaiaiia
</script>
```

2. getAttribute/setAttribute

```html
<body>
<div class="box" id="box1" title="这是title" alt="这是alt" href="这是href" src="这是src" abc="123" qwe="hahaha" zxc="liyang">
<em>123132</em>
</div>
<a  href="123112">123</a>
</body>
<script>
var obox=document.querySelector(".box");

console.log(obox.getAttribute("id")); //heheh
obox.setAttribute("id","abcefh");
console.log(obox.getAttribute("id")); //abcefh

console.log(obox.getAttribute("class"));//xixi
obox.setAttribute("class","ddddd");
console.log(obox.className);  //ddddd

</script>
```


### DOM 增删改

```html
<body>
<div id="box">这是一个div</div>
<h2>zheshi yieg erjibiaoti</h2>
<ul class="list">
<li>link1</li>
<li>link2</li>
<li>link3</li>
</ul>
</body>
<script>

// 创建，插入
var div = document.createElement("div");
document.body.appendChild(div);  //加一个div
div.className = "box";    //加一个class属性
div.innerHTML = "hello";   //内容
console.log(div);

// 删除
var oh2 = document.querySelector(".list");
oh2.remove(); //删除整个.list

var olist = document.querySelector(".list");
var two = olist.children[1]; //删除一个子元素
olist.removeChild(two);


// 修改
var obox = document.getElementById("box");
console.log(obox.outerHTML);
obox.outerHTML = "<span>" + obox.innerHTML + "</span>"; // 将div标签改成了span
</script>
```

