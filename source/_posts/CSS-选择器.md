---
title: CSS选择器
date: 2018-2-20
tags: CSS
author: 映雪
---

天不老，情难绝。心似双丝网，中有千千结。

<!--more-->

### CSS 语法

```css
div{width:200px;height:100px;background:red; }
```

1. 每个 CSS 样式由两部分组成，即选择符和声明，声明又分为属性和属性值
2. 属性必须放在花括号中，属性与属性值用冒号连接。
3. 每条声明用分号结束。
4. 当一个属性有多个属性值的时候，属性值与属性值用空格分隔。
5. 在书写样式过程中，空格、换行等操作不影响属性显示。


### 基础选择器

1. 标签选择器

```css
  <style>
        span {
            color: crimson;
            font-weight: 300;
        }
        p {
            color: blueviolet;
            font-size: large;
        }
 </style>
```

2. id 选择器

- id 选择器选中为标有特定 id 的 HTML 元素

```css
  <style>
        #main {
            color: crimson;
            font-weight: 300;
        }
        #footer {
            color: blueviolet;
            font-size: large;
        }
 </style>
```

3. class 选择器

```css
  <style>
        .box {
            color: crimson;
            font-weight: 300;
        }
        .box2 {
            color: blueviolet;
            font-size: large;
        }
 </style>
```

### 复合选择器

- 复合选择器是建立在基础选择器上，对基本选择器进行组合形成的。

1. 后代选择器

```css
div h3 {
  color: gray;
}
div p {
  background-color: gold;
  color: green;
  font-weight: lighter;
}
```

2. 子元素选择器

```css
  div > p {
​      background-color: blueviolet;
​    }
```

3. 并集选择器

```css
  span , p {
​      background-color: blueviolet;
​    }
```

4. 伪类选择器

```css
/* 选择未访问的链接 */
a:link {
    color: black;
    text-decoration: none;
}
/* 选择已被访问的链接 */
a:visited {
    color: red;
}
/* 选择鼠标滑过的链接 */
a:hover {
    color: royalblue;
}
/* 选择已选中的链接 */
a:active {
    color: saddlebrown;
}

```


### CSS3 关系选择器

1. 子代选择器 选择器 1>选择器 2{}
   选中直接子代 (用 大于号 表示)

```css
.box>span{
   color: red;　　　　　　
　　　　}
```

2. 后代选择器 选择器 1 选择器 2{}
   选中所有后代 (中间用 空格 隔开)

```css
.box span{
　　color: red;
　　　　}
```

3. 相邻兄弟选择器 选择器 1+选择器 2{}
   平级 挨着的后面那一个(用 加号 表示)

```css
.two + p {
  font-size: 20px;
  color: gold;
}
```

4. 通用兄弟选择器 选择器 1~选择器 2{}
   平级 后面所有的兄弟(作用于多个元素，用~表示)

```css
.two ~ p {
  font-size: 20px;
  color: gold;
}
```
