---
title: jQuery HTML
date: 2018-7-26
tags: jQuery
author: 映雪
---

得即高歌失即休，多愁多恨亦悠悠，今朝有酒今朝醉，明日愁来明日愁。

<!--more-->

### jQuery 获取(设置)内容和属性

- 设置(获取)元素内容

1. text() 设置或返回所选元素的文本内容
2. html() 设置或返回所选元素的内容（包括 HTML 标记）
3. val() 设置或返回表单字段的值

```js
$('#btn1').click(function () {
  alert('Text: ' + $('#test').text());
});
$('#btn2').click(function () {
  alert('HTML: ' + $('#test').html());
});
// 设置内容
$('#btn1').click(function () {
  $('#test1').text('Hello world!');
});
$('#btn2').click(function () {
  $('#test2').html('<b>Hello world!</b>');
});
$('#btn3').click(function () {
  $('#test3').val('RUNOOB');
});
```

- 设置(获取)元素属性

1. attr() 设置或返回所选元素的属性值

```js
$('div1').attr();
// 设置属性
$('button').click(function () {
  $('#runoob').attr({
    href: 'http://www.baidu.com',
    title: 'ccc',
  });
});
```

### jQuery 添加元素

1. append() 在被选元素的结尾插入内容
2. prepend() 在被选元素的开头插入内容
3. after() 在被选元素之后插入内容
4. before() 在被选元素之前插入内容

```js
$('p').append('追加文本');
$('p').prepend('在开头追加文本');
$('img').after('在后面添加文本');
$('img').before('在前面添加文本');
```

- 追加元素

```js
function appendText() {
  var txt1 = '<p>文本-1。</p>'; // 使用 HTML 标签创建文本
  var txt2 = $('<p></p>').text('文本-2。'); // 使用 jQuery 创建文本
  var txt3 = document.createElement('p');
  txt3.innerHTML = '文本-3。'; // 使用 DOM 创建文本 text with DOM
  $('body').append(txt1, txt2, txt3); // 追加新元素
}
function afterText() {
  var txt1 = '<b>I </b>'; // 使用 HTML 创建元素
  var txt2 = $('<i></i>').text('love '); // 使用 jQuery 创建元素
  var txt3 = document.createElement('big'); // 使用 DOM 创建元素
  txt3.innerHTML = 'jQuery!';
  $('img').after(txt1, txt2, txt3); // 在图片后添加文本
}
```

### jQuery 删除元素

1. remove()  删除被选元素（及其子元素）
2. empty()   从被选元素中删除子元素

```js
$("#div1").remove()
$("#div1").empty()
```

### jQuery 操作CSS

1. addClass() 向被选元素添加一个或多个类

```js
$("button").click(function(){
  $("h1,h2,p").addClass("blue");
  $("div").addClass("important");
});
```

2. removeClass() 从被选元素删除一个或多个类

```js
$("button").click(function(){
  $("h1,h2,p").removeClass("blue");
});
```

3. toggleClass() 对被选元素进行添加/删除类的切换操作

```js
$("button").click(function(){
  $("h1,h2,p").toggleClass("blue");
});
```

4. css() 设置或返回样式属性

```js
$("p").css("background-color"); // 获取background-color值
$("p").css("background-color","yellow"); // 设置 background-color 值
```

### JQuery 尺寸

- jQuery 提供多个处理尺寸的重要方法

1. width() 方法设置或返回元素的宽度（不包括内边距、边框或外边距）。
2. height() 方法设置或返回元素的高度（不包括内边距、边框或外边距）。
3. innerWidth() 方法返回元素的宽度（包括内边距）。
4. innerHeight() 方法返回元素的高度（包括内边距）。
5. outerWidth() 方法返回元素的宽度（包括内边距和边框）。
6. outerHeight() 方法返回元素的宽度（包括内边距和边框）。


![截屏2022-05-30 上午11.33.27.png](/images/2022/05/30/v2ArOdSKenFuGaT.png)

```js
$("button").click(function(){
  var txt="";
  txt+="div 的宽度是: " + $("#div1").width() + "</br>";
  txt+="div 的高度是: " + $("#div1").height();
  $("#div1").html(txt);
});
$("button").click(function(){
  var txt="";
  txt+="div 宽度，包含内边距: " + $("#div1").innerWidth() + "</br>";
    txt+="div 高度，包含内边距: " + $("#div1").innerHeight();
  $("#div1").html(txt);
});
$("button").click(function(){
  var txt="";
  txt+="div 宽度，包含内边距和边框: " + $("#div1").outerWidth() + "</br>";
  txt+="div 高度，包含内边距和边框: " + $("#div1").outerHeight();
  $("#div1").html(txt);
});
```