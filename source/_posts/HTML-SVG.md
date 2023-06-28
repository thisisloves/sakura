---
title: SVG
date: 2017-2-6
tags: HTML
author: 映雪
---

相顾无相识，长歌怀采薇。

<!-- more-->

### SVG

- SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
- SVG 用于定义用于网络的基于矢量的图形
- SVG 使用 XML 格式定义图形
- SVG 图像在放大或改变尺寸的情况下其图形质量不会有损失
- SVG 是万维网联盟的标准

### SVG 优势

1. SVG 图像可通过文本编辑器来创建和修改
2. SVG 图像可被搜索、索引、脚本化或压缩
3. SVG 是可伸缩的
4. SVG 图像可在任何的分辨率下被高质量地打印
5. SVG 可在图像质量不下降的情况下被放大

### SVG 圆形

- 在 HTML 中 能够将 SVG 元素直接嵌入 HTML 页面中。

```html
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <circle cx="100" cy="50" r="40" stroke="black" stroke-width="2" fill="red" />
</svg>
```

### SVG 五角星

```html
<!DOCTYPE html>
<html>
<body>

<svg xmlns="http://www.w3.org/2000/svg" version="1.1" height="190">
  <polygon points="100,10 40,180 190,60 10,60 160,180"
  style="fill:lime;stroke:purple;stroke-width:5;fill-rule:evenodd;">
</svg>

</body>
</html>
```

### Canvas 与 SVG 的比较

|                       Canvas                       |                          SVG                          |
| :------------------------------------------------: | :---------------------------------------------------: |
|                     依赖分辨率                     |                     不依赖分辨率                      |
|                  不支持事件处理器                  |                    支持事件处理器                     |
|                  弱的文本渲染能力                  |   最适合带有大型渲染区域的应用程序（比如谷歌地图）    |
|        能够以 .png 或 .jpg 格式保存结果图像        | 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快 |
| 最适合图像密集型的游戏，其中的许多对象会被频繁重绘 |                    不适合游戏应用                     |
