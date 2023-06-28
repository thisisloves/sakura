---
title: 回流和重绘
date: 2019-6-23
tags: JS
author: 映雪
---

多情自古伤离别。更那堪，冷落清秋节。

<!--more-->

### 浏览器渲染

![4088852130-5afbe6c95934b.png](/images/2022/03/04/cWgBUHhp1R9yrsb.png)

1. 解析HTML，生成DOM树，解析CSS，生成CSSOM树
2. 将DOM树和CSSOM树结合，生成渲染树(Render Tree)
3. Layout(回流):根据生成的渲染树，进行回流(Layout)，得到节点的几何信息（位置，大小）
4. Painting(重绘):根据渲染树以及回流得到的几何信息，得到节点的绝对像素
5. Display:将像素发送给GPU，展示在页面上。

- 补充：浏览器使用流式布局模型 (Flow Based Layout)，对 Render Tree 的计算通常只需要遍历一次就可以完成，但 table 及其内部元素除外，他们可能需要多次计算，通常要花 3 倍于同等元素的时间，要尽量避免使用table。


### 生成渲染树

![4223770356-5abdb235cdd7d.png](/images/2022/03/04/k7Qy6VObIxBiK2N.png)

1. 从DOM树的根节点开始遍历每个可见节点。
2. 对于每个可见的节点，找到CSSOM树中对应的规则，并应用它们。
3. 根据每个可见节点以及其对应的样式，组合生成渲染树。


### 回流 (Reflow)

- 当 Render Tree 中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为回流。


- 导致回流的操作

1. 添加或删除可见的DOM元素
2. 元素的位置发生变化
3. 元素的尺寸发生变化（包括外边距、内边框、边框大小、高度和宽度等）
4. 内容发生变化，比如文本变化或图片被另一个不同尺寸的图片所替代
5. 页面一开始渲染的时候
6. 浏览器的窗口尺寸变化（因为回流是根据视口的大小来计算元素的位置和大小的）

- 导致回流的属性和方法

```
offsetTop、offsetLeft、offsetWidth、offsetHeight
scrollTop、scrollLeft、scrollWidth、scrollHeight
clientTop、clientLeft、clientWidth、clientHeight
getComputedStyle()
getBoundingClientRect
```

### 重绘 (Repaint)

- 当页面中元素样式的改变并不影响它在文档流中的位置时(例如：color、background-color、visibility 等)，浏览器会将新样式赋予给元素并重新绘制它，这个过程称为重绘。
- 将渲染树的每个节点都转换为屏幕上的实际像素。


### 性能对比

- 回流一定会触发重绘，而重绘不一定会回流。
- 回流比重绘的代价要更高。
- 有时即使仅仅回流一个单一的元素，它的父元素以及任何跟随它的元素也会产生回流。



### 优化

1. 现代的浏览器优化

- 浏览器会维护一个队列，把所有引起回流和重绘的操作放入队列中，如果队列中的任务数量或者时间间隔达到一个阈值的，浏览器就会将队列清空，进行一次批处理，这样可以把多次回流和重绘变成一次。
- 当你访问以下属性或方法时，浏览器会立刻清空队列

```
offsetTop、offsetLeft、offsetWidth、offsetHeight
scrollTop、scrollLeft、scrollWidth、scrollHeight
clientTop、clientLeft、clientWidth、clientHeight
getComputedStyle()
getBoundingClientRect
```

- 因为队列中可能会有影响到这些属性或方法返回值的操作，即使你希望获取的信息与队列中操作引发的改变无关，浏览器也会强行清空队列，确保你拿到的值是最精确的。

2. 代码

1. 避免使用 table 布局。
2. 尽可能在 DOM 树的最末端改变 class。
3. 避免设置多层内联样式。
4. 将动画效果应用到 position 属性为 absolute 或 fixed 的元素上。
5. 避免频繁操作样式，最好一次性重写 style 属性，或者将样式列表定义为 class 并一次性更改 class 属性。
6. 避免频繁操作 DOM，创建一个 documentFragment，在它上面应用所有 DOM 操作，最后再把它添加到文档中。
7. 可以先为元素设置 display: none，操作结束后再把它显示出来。因为在 display 属性为 none 的元素上进行的 DOM 操作不会引发回流和重绘。
8. 避免频繁读取会引发回流/重绘的属性，如果确实需要多次使用，就用一个变量缓存起来。
9. 对具有复杂动画的元素使用绝对定位，使它脱离文档流，否则会引起父元素及后续元素频繁回流。
