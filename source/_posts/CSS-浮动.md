---
title: CSS浮动
date: 2018-2-24
tags: CSS
author: 映雪
---

离人愁、伤别离。碎碎念、深深思。凋零落、吟空悲。续繁华、又何处。

<!--more-->

### CSS 浮动

- 会使元素向左或向右移动，其周围的元素也会重新排列。浮动的框可以向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。由于浮动框不在文档的普通流中，所以文档的普通流中的块框表现得就像浮动框不存在一样。

### CSS 浮动属性

| 属性  |                描述                |           值            |
| :---: | :--------------------------------: | :---------------------: |
| clear |   指定不允许元素周围有浮动元素。   | left,right,none,inherit |
| float | 指定一个盒子（元素）是否可以浮动。 | left,right,none,inherit |

### CSS 浮动样例

1. 框 1 设置 float:right

![截屏2022-06-01 下午5.33.05.png](/images/2022/06/01/cNaRkTQiG6zyDsW.png)


2. 框 1 设置float:right;

![截屏2022-06-01 下午5.33.10.png](/images/2022/06/01/GT4eHnO9Yvxflra.png)

3. 3 个框设置float:left;

![截屏2022-06-01 下午5.32.59.png](/images/2022/06/01/mWPxC7HpX8oun3T.png)

4. 3 个框设置float:lef;但是包含框空间不足

![截屏2022-06-01 下午5.34.00.png](/images/2022/06/01/A3OaiG9oIgVbzht.png)

### 浮动高度塌陷问题

```html
<style>
.news {
  background-color: gray;
  border: solid 1px black;
  }

.news img {
  float: left;
  }

.news p {
  float: right;
  }
</style>
<div class="news">
<img src="news-pic.jpg" />
<p>some text</p>
</div>
```

![截屏2022-06-01 下午5.54.12.png](/images/2022/06/01/LJAV9U25sjFroKy.png)

- 因为浮动元素脱离了文档流，所以包围图片和文本的 div 不占据空间。

- 解决方案

```html
<style>
.news {
  background-color: gray;
  border: solid 1px black;
  }

.news img {
  float: left;
  }

.news p {
  float: right;
  }

.clear {
  clear: both;
  }
</style>
<div class="news">
<img src="news-pic.jpg" />
<p>some text</p>
<div class="clear"></div>
</div>
```

![截屏2022-06-01 下午6.11.30.png](/images/2022/06/01/KZPBw2EmlJ8ODUI.png)

