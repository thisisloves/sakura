---
title: jQuery效果
date: 2018-5-25
tags: jQuery
author: 映雪
---

世界上有两万个你一见钟情的人，但有可能你一生中一个也遇不到。

<!--more-->

### jQuery 显示隐藏

- 通过 jQuery，可以使用 hide() 和 show() 方法来隐藏和显示 HTML 元素。

```js
$('#hide').click(function () {
  $('p').hide();
});

$('#show').click(function () {
  $('p').show();
});
```

### jQuery 淡入淡出

- 通过 jQuery，可以 fadeIn() 和 fadeOut() 实现元素的淡入淡出效果。

```js
$('button').click(function () {
  $('#div1').fadeIn();
  $('#div2').fadeIn('slow');
  $('#div3').fadeIn(3000);
});
$('button').click(function () {
  $('#div1').fadeOut();
  $('#div2').fadeOut('slow');
  $('#div3').fadeOut(3000);
});
```

### jQuery 滑动

1. slideDown() 方法用于向下滑动元素。

```js
$(selector).slideDown(speed, callback);
```

- 可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
- 可选的 callback 参数是滑动完成后所执行的函数名称。

```js
$('#flip').click(function () {
  $('#panel').slideDown();
});
```

2. jQuery slideUp() 方法用于向上滑动元素。

```js
$(selector).slideUp(speed, callback);
```

- 可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
- 可选的 callback 参数是滑动完成后所执行的函数名称。

```js
$('#flip').click(function () {
  $('#panel').slideUp();
});
```

### jQuery 动画

- jQuery animate() 方法用于创建自定义动画。

```js
$(selector).animate({ params }, speed, callback);
```

- animate()方法有三个参数。分别是动画目标(target)，动画执行时间，回调函数。只有第一个参数是必填参数。

```js
$('button').click(function () {
  $('div').animate({
    left: '250px',
    opacity: '0.5',
    height: '150px',
    width: '150px',
  });
});
```

- 如果想要动画按照顺序执行（执行完第一个动画之后，再执行下一个动画）有三种方法：

(1) 借助动画的回调函数。

```js
$('input').click(function () {
  $('div').animate(
    {
      width: '400px',
      height: '400px',
    },
    function () {
      $('div').animate({
        width: '200px',
        height: '200px',
      });
    },
  );
});
```

(2)将动画效果分开写。

```js
$('input').click(function () {
  $('div').animate({
    width: '400px',
    height: '400px',
  });
  $('div').animate({
    width: '200px',
    height: '200px',
  });
});
```

(3) 连缀

```js
$('input').click(function () {
  $('div')
    .animate({
      width: '400px',
      height: '400px',
    })
    .animate({
      width: '200px',
      height: '200px',
    });
});
```

### jQuery 动画停止

- stop()方法是停止动画的方法，他有两个参数，都是布尔值。第一个参数代表是否清除动画队列，第二个参数代表是否直接运行到最后结果。

```js
$('#stop').click(function () {
  $('#panel').stop();
});
```

### jQuery 动画递归

```js
$('div').slideToggle(2000, recursion);
function recursion() {
  $(this).slideToggle(2000, recursion);
}
```

- 自我调用

```js
$('div').slideToggle(2000, function () {
  $(this).slideToggle(2000, arguments.callee);
});
```

- arguments.callee：常用在匿名函数中， 代表当前的函数。

### jQuery 动画全局方法

- $.fx.interval 属性可以调整动画每秒的运行帧数，默认为 13 毫秒。数字越小越流畅，但可能影响程序性能。
- $.fx.interval=1000;动画的帧数设置。
- $.fx.off=true;动画关闭。
