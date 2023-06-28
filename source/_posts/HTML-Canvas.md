---
title: Canvas
date: 2017-2-5
tags: HTML
author: 映雪
---

待浮花浪蕊俱尽，伴君幽独。

<!--more-->

### Canvas (画布)

- canvas 标签是 HTML5 中的新标签，像所有的 dom 对象一样它有自己本身的属性、方法和事件。操作 canvas 的 javascript API，它可以实现在网页中完成动态的 2D 与 3D 图像技术。


### 创建画布（Canvas）

```html
<canvas id="myCanvas" width="200" height="100" style="border:1px solid #000000;"> </canvas>
```

### 绘制直线

```js
// 为画布设置一根画笔
let ctx = document.querySelector('#myCanvas').getContext('2d');
//1.确定直线的起点:ctx.moveTo(x,y);,将画笔移动到(x,y)坐标位置。
ctx.moveTo(50, 50);
// 2.确定直线的端点:ctx.lineTo(x,y);
ctx.lineTo(65, 50);
ctx.lineTo(100, 70);
ctx.lineTo(100, 70);
ctx.lineTo(110, 70);
ctx.lineTo(145, 50);
ctx.lineTo(160, 50);
ctx.lineTo(160, 75);
ctx.lineTo(150, 85);
ctx.lineTo(160, 95);
ctx.lineTo(160, 120);
ctx.lineTo(145, 120);
ctx.lineTo(110, 100);
ctx.lineTo(100, 100);
ctx.lineTo(65, 120);
ctx.lineTo(50, 120);
ctx.lineTo(50, 95);
ctx.lineTo(60, 85);
ctx.lineTo(50, 75);
// 3.将最后一个端点和起点连接起来进行封口
ctx.closePath();
//4.为画笔设置边线颜色:
ctx.strokeStyle = '#3385ff';
//5.实现确定的起点和端点之间连线的绘制
ctx.stroke();
//6.为画笔设置填充颜色
ctx.fillStyle = '#e1565b';
//7.实现对封闭图形的颜色填充
ctx.fill();
```

### 绘制矩形

```js
//方法1：绘制边线矩形
ctx.strokeRect(200, 50, 250, 150);
// 方法2：绘制填充矩形
ctx.fillRect(200, 250, 250, 80);
//方法3：绘制任意矩形(该方法需要实现确定的起点和端点之间连线的绘制)
ctx.rect(470, 40, 100, 250);
ctx.stroke();
```

### 绘制圆

```js
ctx.arc(cx, cy, radius, startAngle, endAngle, counterClockwise);
```

|       参数       |                                   解释                                   |
| :--------------: | :----------------------------------------------------------------------: |
|     (cx,cy)      |                              圆的圆心坐标。                              |
|      radius      |                                 圆的半径                                 |
|    startAngle    |                         弧的起点角度，单位为弧度                         |
|     endAngle     |                         弧的端点角度，单位为弧度                         |
| counterClockwise | 设置弧的方向，默认值为 false，表示顺时针方向。取值为 true 表示逆时针方向 |

```js
ctx.arc(canvas.width / 2, canvas.height / 2, 200, 0, Math.PI / 2, false);
ctx.stroke();
```

### Canvas 文本

|      属性和方法      |            解释            |
| :------------------: | :------------------------: |
|         font         |          定义字体          |
|  fillText(text,x,y)  | 在 canvas 上绘制实心的文本 |
| strokeText(text,x,y) | 在 canvas 上绘制空心的文本 |

```js
var c = document.getElementById('myCanvas');
var ctx = c.getContext('2d');
ctx.font = '30px Arial';
ctx.strokeText('Hello World', 10, 50);
```

### Canvas 渐变

1. createLinearGradient(x,y,x1,y1) - 创建线条渐变

```js
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
 
// 创建渐变
var grd=ctx.createLinearGradient(0,0,200,0);
grd.addColorStop(0,"red");
grd.addColorStop(1,"white");
 
// 填充渐变
ctx.fillStyle=grd;
ctx.fillRect(10,10,150,80);
```

2. createRadialGradient(x,y,r,x1,y1,r1) - 创建一个径向/圆渐变

```js
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
 
// 创建渐变
var grd=ctx.createRadialGradient(75,50,5,90,60,100);
grd.addColorStop(0,"red");
grd.addColorStop(1,"white");
 
// 填充渐变
ctx.fillStyle=grd;
ctx.fillRect(10,10,150,80);
```

### Canvas 图像

- 把一幅图像放置到画布上使用，drawImage(image,x,y)

```js
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
var img=document.getElementById("scream");
ctx.drawImage(img,10,10);
```

### 注意

- Internet Explorer 8 以及更早的版本不支持 `<canvas>` 标签。Internet Explorer 9+, Firefox, Opera, Chrome 以及 Safari 支持 `<canvas> `标签。