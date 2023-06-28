---
title: 网格布局
date: 2019-1-26
tags: CSS
author: 映雪
---

独守寒窗月漫漫，静思春夜雨蒙蒙。我自流连随风笑，凡人痴梦各不同。

<!--more-->


### Grid 布局

- 将网页划分成一个个网格，可以任意组合不同的网格，做出各种各样的布局。Flex 布局是轴线布局，只能指定"项目"针对轴线的位置，可以看作是一维布局。Grid 布局则是将容器划分成"行"和"列"，产生单元格，然后指定"项目所在"的单元格，可以看作是二维布局。Grid 布局远比 Flex 布局强大。

```css
.box{
  display: grid;
}
```

![截屏2022-06-06 上午11.26.21.png](/images/2022/06/06/mvdnbuol1CPWELi.png)


### Grid 容器属性

1. **display** 

- grid 指定一个容器采用网格布局

```css
div{
	display:'grid';
}
```

- 默认情况下，容器元素都是块级元素，但是也可以是行内元素。

```css
div{
	display:'inline-grid'
}
```

- 设置为网格布局后，容器的float，display:'inline' 等设置都将失效


2. **grid-template-columns 和 grid-template-rows**

* 容器指定了网格布局后，接着就要划分行和列。grid-template-columns 属性指定每一列的列宽。grid-template-rows 指定定义每一行的行高。

```css
container{
	displat:'grid';
	grid-template-columns:100px 100px 100px;
	grid-template-rows:100px 100px 100px;
}
```

![截屏2022-06-06 上午11.32.12.png](/images/2022/06/06/stWbeZiQOKgExLI.png)

* 除了绝对单位也可以使用百分比单位

```css
container{
	display:'grid';
	grid-template-columns:33.33% 33.33% 33.33% ;
	grid-template-rows:33.33% 33.33% 33.33%;
}
```

* repeat()

* 有时候，重复写同样的值非常麻烦，尤其网格很多时。这时，可以使用repeat()函数，简化重复的值。上面的代码用repeat()改写如下。

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}
```

* auto-fill 关键字

* 有时，单元格的大小是固定的，但是容器的大小不确定。如果希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用auto-fill关键字表示自动填充。

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}
```

* fr 关键字

* 为了方便表示比例关系，网格布局提供了fr关键字（fraction 的缩写，意为"片段"）。如果两列的宽度分别为1fr和2fr，就表示后者是前者的两倍。


```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

* fr可以与绝对长度的单位结合使用，这时会非常方便。

```css
.container {
  display: grid;
  grid-template-columns: 150px 1fr 2fr;
}
```

* auto 关键字 
- 表示由浏览器自己决定长度。

```css
grid-template-columns: 100px auto 100px;
```

* 第二列的宽度，基本上等于该列单元格的最大宽度，除非单元格内容设置了min-width，且这个值大于最大宽度。


3. **grid-row-gap 和 grid-columns-gap** 

* grid-row-gap  属性设置行与行之间的间距 和 grid-columns-gap 属性设置列与列之间的间距。

```css
.container{
  grid-row-gap: 20px;
  grid-column-gap: 20px;
}
```

![截屏2022-06-06 上午11.39.53.png](/images/2022/06/06/1pSy3jGuCrwfPEU.png)

- grid-row-gap用于设置行间距，grid-column-gap用于设置列间距。



3. **grid-template-areas**

- 网格布局允许指定"区域"（area），一个区域由单个或多个单元格组成。grid-template-areas属性用于定义区域。

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';
}

```

- 上面代码先划分出9个单元格，然后将其定名为a到i的九个区域，分别对应这九个单元格。
- 多个单元格合并成一个区域的写法如下。


```css
.container {
grid-template-areas: 'a a a'
                     'b b b'
                     'c c c';}
```


- 注意，区域的命名会影响到网格线。每个区域的起始网格线，会自动命名为区域名-start，终止网格线自动命名为区域名-end。比如，区域名为header，则起始位置的水平网格线和垂直网格线叫做header-start，终止位置的水平网格线和垂直网格线叫做header-end。


4. **grid-auto-flow**

* 划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行。

```css
.container{
	grid-auto-flow:row;
}
```

![截屏2022-06-06 上午11.32.12.png](/images/2022/06/06/stWbeZiQOKgExLI.png)

* 也可以设置成为先列后行

```css
.conatiner{
	grid-auto-flow:column;
}
```

![截屏2022-06-06 上午11.57.41.png](/images/2022/06/06/E3NB2dQLpJblesj.png)

5. **justify-items 和 align-items**

- justify-items属性设置单元格内容的水平位置（左中右），align-items属性设置单元格内容的垂直位置（上中下）。

```css
.container{
	justify-items: start | end | center | stretch;
	align-items: start | end | center | stretch;
}
```

* start：对齐单元格的起始边缘。
* end：对齐单元格的结束边缘。
* center：单元格内部居中。
* stretch：拉伸，占满单元格的整个宽度（默认值）


```css
.container {
  justify-items: start;
}

```

![截屏2022-06-06 下午12.00.14.png](/images/2022/06/06/pk741OWgTRGutJP.png)

```css
.container {
  align-items: start;
}
```

![截屏2022-06-06 下午12.00.37.png](/images/2022/06/06/WdfkTOtnv1MQi4j.png)



6. **justify-content 和align-content**

- ›justify-content属性是整个内容区域在容器里面的水平位置（左中右），align-content属性是整个内容区域的垂直位置（上中下）。

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```

- center - 容器内部居中。

![截屏2022-06-06 下午2.29.42.png](/images/2022/06/06/MBm9Wt2hFyTKlgn.png)

- space-around - 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍。

![截屏2022-06-06 下午2.30.12.png](/images/2022/06/06/5ug3VCwI4sPeQSD.png)

- space-between - 项目与项目的间隔相等，项目与容器边框之间没有间隔。

![截屏2022-06-06 下午2.30.05.png](/images/2022/06/06/OspaDxr8jcL5dQZ.png)

7. **grid-auto-columns 和 grid-auto-rows** 

- grid-auto-columns属性和grid-auto-rows属性用来设置，浏览器自动创建的多余网格的列宽和行高。它们的写法与grid-template-columns和grid-template-rows完全相同。如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。


```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-auto-rows: 50px; 
}
```

![截屏2022-06-06 下午2.58.24.png](/images/2022/06/06/Rl96mMyDCbAXgdq.png)


### 项目属性

1. **grid-column-start 属性，grid-column-end 属性，grid-row-start 属性，grid-row-end 属性**

- grid-column-start属性：左边框所在的垂直网格线
- grid-column-end属性：右边框所在的垂直网格线
- grid-row-start属性：上边框所在的水平网格线
- grid-row-end属性：下边框所在的水平网格线

```css
.item-1 {
  grid-column-start: 2;
  grid-column-end: 4;
}
```

![截屏2022-06-06 下午3.19.30.png](/images/2022/06/06/cAbein83FwXHPJm.png)


2. **grid-area属性**

- grid-area属性指定项目放在哪一个区域。

```css

.item-1 {
  grid-area: e;
}
```

![截屏2022-06-06 下午3.15.27.png](/images/2022/06/06/ChsLvNqnDXeAgft.png)


3. **justify-self 属性，align-self 属性，place-self 属性**

- justify-self属性设置单元格内容的水平位置（左中右），跟justify-items属性的用法完全一致，但只作用于单个项目。
- align-self属性设置单元格内容的垂直位置（上中下），跟align-items属性的用法完全一致，也是只作用于单个项目。

```
.item {
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```


- start：对齐单元格的起始边缘。
- end：对齐单元格的结束边缘。
- center：单元格内部居中。
- stretch：拉伸，占满单元格的整个宽度（默认值）。


```css
.item-1  {
  justify-self: start;
}
```

![c.png](/images/2022/06/06/i7q1WVLOfDlvjgF.png)