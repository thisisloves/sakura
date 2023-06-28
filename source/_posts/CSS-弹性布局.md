---
title: 弹性布局
date: 2019-1-23
tags: CSS
author: 映雪
---

看那天地日月，恒静无言；青山长河，世代绵延；就像在我心中，你从未离去，也从未改变。

<!--more-->

### Flex 布局

- Flex 是 Flexible Box 的缩写，主要解决某个元素中子元素的布局方式，它为布局提供了强大的灵活性。
- 任何一个容器都可以指定为 Flex 布局。

```css
.box{
  display: flex;
}
```

![截屏2022-06-02 下午5.49.03.png](/images/2022/06/02/nyuQ7WqZpr5aJlG.png)


### Flex 容器属性

1. **flex-direction**

- flex-direction属性决定主轴的方向（即项目的排列方向）。 

```css
.box {
  flex-direction: row | row-reverse | column | column-reverse;  
}
```

![截屏2022-06-02 下午5.57.42.png](/images/2022/06/02/eNJzhj2uMkocnFP.png)

2. **flex-wrap**

- 默认情况下项目都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。

```css
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

![截屏2022-06-02 下午6.01.00.png](/images/2022/06/02/wsVgAa98nSibcdP.png)
![截屏2022-06-02 下午6.01.05.png](/images/2022/06/02/Lnwl67Zy1JGue2d.png)
![截屏2022-06-02 下午6.01.10.png](/images/2022/06/02/SxZWhsMq9U3Q2jz.png)

3. **justify-content**

- justify-content属性定义了项目在主轴上的对齐方式。

```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

![截屏2022-06-02 下午6.25.30.png](/images/2022/06/02/mVFvwDpYjXOkNfc.png)


4. **align-items**

- align-items属性定义项目在交叉轴上如何对齐。

```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

![截屏2022-06-02 下午6.33.40.png](/images/2022/06/02/XLAkBN2pmM9CrVg.png)


5. **align-content**

- align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

```css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

![截屏2022-06-02 下午6.35.37.png](/images/2022/06/02/KvyI6j4MuQ5gef7.png)


### Flex 项目属性

1. **order属性**

- order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

```css
.item {
  order: <integer>;
}
```

![截屏2022-06-02 下午6.39.32.png](/images/2022/06/02/IcVHLiUu3vh4ZXm.png)

2. **flex-grow属性**

- flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

```css
.item {
  flex-grow: <number>; /* default 0 */
}
```

![截屏2022-06-02 下午6.41.51.png](/images/2022/06/02/ZyzeBdno8MUQ2xt.png)


3. **flex-shrink属性**

- flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

```css
.item {
  flex-shrink: <number>; /* default 1 */
}

```

4. **flex-basis属性**

- flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

```css
.item {
  flex-basis: <length> | auto; /* default auto */
}
```

5. **flex属性**

- flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。

```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

6. **align-self属性**

- align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

```css

.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

![截屏2022-06-02 下午6.47.24.png](/images/2022/06/02/3rgAfHlx8tmFEKh.png)