---
title: BFC
date: 2018-3-15
tags: CSS
author: 映雪
---

你如花美眷，也敌不过似水流年。那首关于我们的歌，我把结局唱给你听，你若渡我一生苦海，我愿许你一世繁华。

<!--more-->

### BFC 块格式化上下文 (Block Formatting Context)

- 浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells, 和 table-captions），以及 overflow 值不为“visiable”的块级盒子，都会为他们的内容创建新的 BFC（块级格式上下文）。

### BFC 规则

1. 内部的 box 会在垂直方向，一个接一个地放置（可以看作 BFC 中有一个的常规流）。
2. Box 垂直方向的距离有 margin 决定。属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠
3. 每个元素的 margin box 的左边，会包含块 border box 的左边相接触（对于从左往右的格式化，否则相反），即使存在浮动也是如此
4. BFC 的区域不会与 float box 重叠
5. BFC 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此
6. 计算 BFC 的高度时，浮动元素也参与计算

### BFC 产生

1. 根元素或其它包含它的元素
2. 浮动 (元素的 float 不为 none)
3. 绝对定位元素 (元素的 position 为 absolute 或 fixed)
4. 行内块 inline-blocks (元素的 display: inline-block)
5. 表格单元格 (元素的 display: table-cell，HTML 表格单元格默认属性)
6. 表格标题 (元素的 display: table-caption, HTML 表格标题默认属性)
7. overflow 的值不为 visible 的元素
8. 弹性盒 flex boxes (元素的 display: flex 或 inline-flex)

### 高度塌陷

- 父元素高度自适应，子元素 float 后，造成父元素高度为 0，称为高度塌陷问题。
- float 脱离了普通流，并且创建了新的 BFC，而父元素不具备产生 BFC 的条件，所以它的高度为 0。

1. 使用 BFC 解决高度坍塌

- BFC 会把它包含的浮动元素高度也算在里面，也就是闭合浮动。
- overflow: auto 并不会闭合浮动，而是 overflow: auto 会创建一个新的 BFC，避免浮动的元素侵入其他元素。

### BFC 常见作用

1. 阻止外边距折叠

- margin 塌陷问题：在标准文档流中，块级标签之间竖直方向的 margin 会以大的为准，这就是 margin 的塌陷现象。可以用 overflow：hidden 产生 bfc 来解决。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
<head>
<style>
div{
    width: 100px;
    height: 100px;
    background: lightblue;
    margin: 100px;
}
</style>
</head>
<body>
    <div></div>
    <div></div>
</body>
</html>
```
![截屏2022-06-02 下午5.15.06.png](/images/2022/06/02/wvKfrGn7W61ABHe.png)



- 两个 div 元素都处于同一个 BFC 容器下（这里指 body 元素），所以第一个 div 的下边距和第二个 div 的上边距发生了重叠，所以两个盒子之间距离只有 100px，而不是 200px。
- 解决方案:放在不同的 BFC 容器中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
<head>
<style>
.container {
    overflow: hidden;
}
p {
    width: 100px;
    height: 100px;
    background: lightblue;
    margin: 100px;
}
</style>
</head>
<body>
 <div class="container">
    <p></p>
</div>
<div class="container">
    <p></p>
</div>

</body>
</html>
```

![截屏2022-06-02 下午5.22.16.png](/images/2022/06/02/W3lJOgmKSkGsdYi.png)


2. 包含浮动元素

- 高度塌陷问题，在通常情况下父元素的高度会被子元素撑开，而在这里因为其子元素为浮动元素所以父元素发生了高度坍塌，上下边界重合，这时就可以用 BFC 来清除浮动了。

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">

<body>

<div style="border: 1px solid blue;">
 <div style="width: 100px;height: 100px;background: red;float: left;">

 </div>

</div>

</body>
</html>
```

![截屏2022-06-02 下午5.22.34.png](/images/2022/06/02/wBqNjgrCPdWZzKX.png)

- 由于容器内元素浮动，脱离了文档流，所以容器只剩下 2px 的边距高度。如果触发容器的 BFC，那么容器将会包裹浮动元素。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
<body>
<div style="border: 1px solid blue;overflow: hidden">
    <div style="width: 100px;height: 100px;background: red;float: left;">
    </div>

</div>

</body>
</html>
```

![截屏2022-06-02 下午5.22.38.png](/images/2022/06/02/UCKH4hlNmRwkFiq.png)


3. 阻止元素被浮动元素覆盖

- 由于左侧块级元素发生了浮动，所以和右侧未发生浮动的块级元素不在同一层内，所以会发生 div 遮挡问题。可以给右侧元素添加 overflow: hidden，触发 BFC 来解决遮挡问题。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">

<body>

<div style="height: 100px;width: 100px;float: left;background: lightblue">
我是一个左浮动的元素
</div>

<div style="width: 200px; height: 200px;background: pink">
我是一个没有设置浮动, 也没有触发 BFC 元素, 现在看看会发生什么效果吧，要仔细看。
</div>


</body>
</html>
```

![截屏2022-06-02 下午5.22.45.png](/images/2022/06/02/37rpLuf61DFGszo.png)

- 这时候其实第二个元素有部分被浮动元素所覆盖，但是文本信息不会被浮动元素所覆盖，如果想避免元素被覆盖，可触发第二个元素的 BFC 特性，在第二个元素中加入 overflow：hidden，就会变成：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
<body>

<div style="height: 100px;width: 100px;float: left;background: lightblue">
我是一个左浮动的元素
</div>

<div style="width: 200px; height: 200px;background: pink;overflow:hidden">
我是一个没有设置浮动, 也没有触发 BFC 元素, 现在看看会发生什么效果吧，要仔细看。
</div>

</body>
</html
```

![截屏2022-06-02 下午5.22.49.png](/images/2022/06/02/roZf796GQ8qAkBt.png)