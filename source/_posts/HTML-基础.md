---
title: HTML
date: 2017-2-1
tags: HTML
author: 映雪
---

染火枫林，琼壶歌月，长歌倚楼。岁岁年年，花前月下，一尊芳酒。水落红莲，唯闻玉磬，但此情依旧。

<!--more-->

### HTML

- 超文本标记语言（英语：HyperText Markup Language，简称：HTML）是一种用于创建网页的标准标记语言。可以使用 HTML 来建立自己的 WEB 站点，HTML 运行在浏览器上，由浏览器来解析。

### HTML 组成

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>HTML</title>
  </head>
  <body>
    <h1>我的第一个标题</h1>
    <p>我的第一个段落。</p>
  </body>
</html>
```

![截屏2022-05-31 上午10.05.08.png](/images/2022/05/31/Mtgy4xo7ZCmHTnO.png)

1. `<!DOCTYPE html>` 声明为 HTML5 文档
2. `<html>` 元素是 HTML 页面的根元素
3. `<head>` 元素包含了文档的元（meta）数据，如 <meta charset="utf-8"> 定义网页编码格式为 utf-8。
4. `<title>` 元素描述了文档的标题
5. `<body>` 元素包含了可见的页面内容
6. `<h1>` 元素定义一个大标题
7. `<p>` 元素定义一个段落

### HTML 标签

- HTML 标记标签通常被称为 HTML 标签 (HTML tag)。

1. HTML 标签是由尖括号包围的关键词，比如 `<html>`
2. HTML 标签通常是成对出现的，比如 `<b>` 和 `</b>`
3. 标签对中的第一个标签是开始标签，第二个标签是结束标签
4. 开始和结束标签也被称为开放标签和闭合标签

```html
<p></p>
```

### HTML 元素

- "HTML 标签" 和 "HTML 元素" 通常都是描述同样的意思。
- 但是严格来讲, 一个 HTML 元素包含了开始标签与结束标签。

```html
<p>这是一个段落</p>
```

### HTML 网页结构

![截屏2022-05-31 上午10.06.23.png](/images/2022/05/31/HdvyDExL8QfUXmK.png)

### HTML 版本

|   版本    | 发布时间 |
| :-------: | :------: |
|   HTML    |   1991   |
|   HTML+   |   1993   |
| HTML 2.0  |   1995   |
| HTML 3.2  |   1997   |
| HTML 4.01 |   1999   |
| XHTML 1.0 |   2000   |
|   HTML5   |   2012   |
|  XHTML5   |   2013   |


### <!DOCTYPE> 声明

- <!DOCTYPE>声明有助于浏览器中正确显示网页。
- 网络上有很多不同的文件，如果能够正确声明HTML的版本，浏览器就能正确显示网页内容。
- doctype 声明是不区分大小写的，以下方式均可：

```html
<!DOCTYPE html>
<!DOCTYPE HTML>
<!doctype html>
<!Doctype Html>
```

### 通用声明

1. HTML5

```html
<!DOCTYPE html>
```

2. HTML 4.01

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
```

3. XHTML 1.0

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```