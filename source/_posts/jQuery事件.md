---
title: jQuery事件
date: 2018-5-1
tags: jQuery
author: 映雪
---

烟水茫茫，千里斜阳暮。山无数，乱红如雨，不记来时路。

<!--more-->

### 常见 DOM 事件

|  鼠标事件  | 键盘事件 | 表单事件 | 文档/窗口事件 |
| :--------: | :------: | :------: | :-----------: |
|   click    | keypress |  submit  |     load      |
|  dblclick  | keydown  |  change  |    resize     |
| mouseenter |  keyup   |  focus   |    scroll     |
| mouseleave |          |   blur   |    unload     |
|   hover    |          |          |               |

### jQuery 事件

jQuery 是为事件处理特别设计的。
juqery 事件就是将 JavaScrit 事件简写(省略了 on)，然后在后面加上括号。(注：前缀必须是 jQuery 对象)

```js
document.onclick = function () {
  $(document).click(function () {
    //事件处理函数
  });
};
```

### jQuery 事件绑定

1. 同时绑定多个事件，共用一个事件处理函数

```js
$('input').bind('mouseover mouseout', function () {
  alert(1);
});
```

2. 同时绑定多个事件，并使用不同的事件处理函数

```js
$('input').bind({
    mouseout:function(){alert('移除')}
},
    mouseover:function(){alert('移入')}
)
```

3. 同一事件绑定不同的事件处理函数

```js
$('input').bind('click', fn1);
$('input').bind('click', fn2);
function fn1() {
  alert(1);
}
function fn2() {
  alert(2);
}
//执行顺序为事件绑定的顺序
```

4. 事件的命名空间

- 方便模拟事件、解绑事件时选定特定事件。

```js
$('input').bind('click', function () {
  alert(1);
});
$('input').bind('click.abc', function () {
  alert(1);
});
```

### 事件解绑

- 不传参数的时候默认移除该 DOM 元素上的所有的事件

```js
$('input').unbind();
```

- 移出该 DOM 元素上参数('mouseover')的事件

```
$('input').unbind('mouseover')
```

### 模拟事件

- 模拟用户触发事件：同时触发事件冒泡。

```js
$('input').triggerHandler('click')(jquery对象).triggerHandler方法(事件名);
```

- 模拟用户触发事件：不触发事件冒泡。
- 计时器自动执行事件时可用

### 事件委托

- 什么是事件委托：有 100 个学生同时在某天中午收到快递，但这 100 个学生不可能同时站在学校门口等，那么都会委托门卫去收取，然后再逐个交给学生。 而在 jQuery 中，我们通过事件冒泡的特性，让子元素绑定的事件冒泡到父元素(或祖先元素) 上，然后再进行相关处理即可。
- 传统事件绑定，会让性能变得很低，造成程序中非常多的冗余，事件委托完美解决了这个问题，他只需要给父级元素绑定一个事件，那么他的自己元素全部会在事件流中接受到事件，并对事件处理函数做出相应。

- 一个新的方法 on(on 已经全面取代了 bind)

```js
$('div').on('click', 'input', function (evt) {
  $('div').append($('input').eq(0).clone(true));
});
```

### triggerHandler()方法

- 鼠标放在上面每 1000 毫秒执行一次

```html
<style>
    .box{width:200px;height: 200px;background: red;}
</style>
</head>
<body>
<div class="box"></div>
</body>
<script src="https://cdn.bootcss.com/jquery/2.2.4/jquery.js"></script>-->
<script>
$(".box").click(function(){
    console.log(123);
})
setInterval(()=>{
    $(".box").triggerHandler("click");//鼠标放在上面每1000毫秒执行一次
},1000)
</script>
```
