---
title: 媒体查询
date: 2018-2-25
tags: CSS
author: 映雪
---

一点朱砂，两方罗帕，三五鸿雁，乱了四季扬花。六弦绿漪，七星当挂，八九分相思，懒了十年琵琶。

<!--more-->

### 媒体查询(@media)

- 媒体查询可以让我们根据设备显示器的特性（如视口宽度、屏幕比例、设备方向：横向或纵向）为其设定 CSS 样式，媒体查询由媒体类型和一个或多个检测媒体特性的条件表达式组成。媒体查询中可用于检测的媒体特性有 width 、 height 和 color （等）。使用媒体查询，可以在不改变页面内容的情况下，为特定的一些输出设备定制显示效果。

### @media 语法

```css
@media not|only mediatype and (mediafeature and|or|not mediafeature) {
  CSS-Code;
}
```

### @media 类型

|  媒体类型  |                          描述                          |
| :--------: | :----------------------------------------------------: |
|    all     |                  用于所有的媒体设备。                  |
|   aural    |                 用于语音和音频合成器。                 |
|  braille   |             用于盲人用点字法触觉回馈设备。             |
|  embossed  |             用于分页的盲人用点字法打印机。             |
|  handheld  |                  用于小的手持的设备。                  |
|   print    |                      用于打印机。                      |
| projection |               用于方案展示，比如幻灯片。               |
|   screen   |                    用于电脑显示器。                    |
|    tty     | 用于使用固定密度字母栅格的媒体，比如电传打字机和终端。 |
|     tv     |                 用于电视机类型的设备。                 |

```css
@media screen
{
    p.test {font-family:verdana,sans-serif;font-size:14px;}
}
@media print
{
    p.test {font-family:times,serif;font-size:10px;}
}
@media screen,print
{
    p.test {font-weight:bold;}
```

### @media 逻辑操作运算符

- not , and , 和 only 可用于联合构造复杂的媒体查询，可以通过用逗号分隔多个媒体查询，将它们组合为一个规则。

1. not: not 运算符用于否定媒体查询，如果不满足这个条件则返回 true，否则返回 false。 如果出现在以逗号分隔的查询列表中，它将仅否定应用了该查询的特定查询。 如果使用 not 运算符，则还必须指定媒体类型。
2. only: only 运算符仅在整个查询匹配时才用于应用样式，并且对于防止较早的浏览器应用所选样式很有用。 当不使用 only 时，旧版本的浏览器会将 screen and (max-width: 500px) 简单地解释为 screen，忽略查询的其余部分，并将其样式应用于所有屏幕。 如果使用 only 运算符，则还必须指定媒体类型。
3. , (逗号) 逗号用于将多个媒体查询合并为一个规则。 逗号分隔列表中的每个查询都与其他查询分开处理。 因此，如果列表中的任何查询为 true，则整个 media 语句均返回 true。 换句话说，列表的行为类似于逻辑或 or 运算符。
4. and: and 操作符用于将多个媒体查询规则组合成单条媒体查询，当每个查询规则都为真时则该条媒体查询为真，它还用于将媒体功能与媒体类型结合在一起。

```css
.example {
  padding: 20px;
  color: white;
}
/* 超小设备 (手机, 600px 以下屏幕设备) */
@media only screen and (max-width: 600px) {
  .example {
    background: red;
  }
}

/* 小设备 (平板电脑和大型手机，600 像素及以上) */
@media only screen and (min-width: 600px) {
  .example {
    background: green;
  }
}

/* 中型设备（平板电脑，768 像素及以上） */
@media only screen and (min-width: 768px) {
  .example {
    background: blue;
  }
}

/* 大型设备（笔记本电脑/台式机，992 像素及以上） */
@media only screen and (min-width: 992px) {
  .example {
    background: orange;
  }
}

/* 超大型设备（大型笔记本电脑和台式机，1200 像素及以上） */
@media only screen and (min-width: 1200px) {
  .example {
    background: pink;
  }
}
```
