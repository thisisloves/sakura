---
title: CSS转换
date: 2018-3-13
tags: CSS
author: 映雪
---

霜雪明，照流星；千古事，任侠行。

<!--more-->

### CSS 转换

- CSS 转换可以对元素进行移动、缩放、转动、拉长或拉伸。

### 2D 转换

1. translate() 方法

- translate()方法，根据左(X 轴)和顶部(Y 轴)位置给定的参数，从当前元素位置移动。

```css
div {
  transform: translate(50px, 100px);
  -ms-transform: translate(50px, 100px); /* IE 9 */
  -webkit-transform: translate(50px, 100px); /* Safari and Chrome */
}
```

2. rotate() 方法

- rotate()方法，在一个给定度数顺时针旋转的元素。负值是允许的，这样是元素逆时针旋转。

```css
div {
  transform: rotate(30deg);
  -ms-transform: rotate(30deg); /* IE 9 */
  -webkit-transform: rotate(30deg); /* Safari and Chrome */
}
```

3. scale() 方法

- cale()方法，该元素增加或减少的大小，取决于宽度（X 轴）和高度（Y 轴）的参数。

```css
div {
  -ms-transform: scale(2, 3); /* IE 9 */
  -webkit-transform: scale(2, 3); /* Safari */
  transform: scale(2, 3); /* 标准语法 */
}
```

4. skew() 方法

- 包含两个参数值，分别表示 X 轴和 Y 轴倾斜的角度，如果第二个参数为空，则默认为 0，参数为负表示向相反方向倾斜。

```css
div {
  transform: skew(30deg, 20deg);
  -ms-transform: skew(30deg, 20deg); /* IE 9 */
  -webkit-transform: skew(30deg, 20deg); /* Safari and Chrome */
}
```

### 3D 转换

1. rotateX() 方法

- rotateX()方法，围绕其在一个给定度数 X 轴旋转的元素。

```css
div {
  transform: rotateX(120deg);
  -webkit-transform: rotateX(120deg); /* Safari 与 Chrome */
}
```

2. rotateY() 方法

- rotateY()方法，围绕其在一个给定度数 Y 轴旋转的元素。

```css
div {
  transform: rotateY(130deg);
  -webkit-transform: rotateY(130deg); /* Safari 与 Chrome */
}
```
