---
title: CSS动画
date: 2018-3-12
tags: CSS
author: 映雪
---

人生就像一场舞会，教会你最初舞步的人却未必能陪你走到散场。

<!--more-->

### 过渡动画

- 是元素从一种样式逐渐改变为另一种的效果。

|            属性            |                     描述                     |
| :------------------------: | :------------------------------------------: |
|         transition         | 简写属性，用于在一个属性中设置四个过渡属性。 |
|    transition-property     |       规定应用过渡的 CSS 属性的名称。        |
|    transition-duration     |      定义过渡效果花费的时间。默认是 0。      |
| transition-timing-function |   规定过渡效果的时间曲线。默认是 "ease"。    |
|      transition-delay      |       规定过渡效果何时开始。默认是 0。       |

```css
div {
  transition-property: width;
  transition-duration: 1s;
  transition-timing-function: linear;
  transition-delay: 2s;
  /* Safari */
  -webkit-transition-property: width;
  -webkit-transition-duration: 1s;
  -webkit-transition-timing-function: linear;
  -webkit-transition-delay: 2s;
}
```

### CSS 动画(@keyframes)

- 可以改变任意多的样式任意多的次数。最好用百分比来规定变化发生的时间，或用关键词 "from" 和 "to"，等同于 0% 和 100%。0% 是动画的开始，100% 是动画的完成。

- 动画属性

|           属性            |                                           描述                                           |
| :-----------------------: | :--------------------------------------------------------------------------------------: |
|        @keyframes         |                                        规定动画。                                        |
|         animation         |                                 所有动画属性的简写属性。                                 |
|      animation-name       |                               规定 @keyframes 动画的名称。                               |
|    animation-duration     |                     规定动画完成一个周期所花费的秒或毫秒。默认是 0。                     |
| animation-timing-function |                           规定动画的速度曲线。默认是 "ease"。                            |
|    animation-fill-mode    | 规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。 |
|      animation-delay      |                               规定动画何时开始。默认是 0。                               |
| animation-iteration-count |                             规定动画被播放的次数。默认是 1。                             |
|    animation-direction    |                   规定动画是否在下一周期逆向地播放。默认是 "normal"。                    |
|   animation-play-state    |                      规定动画是否正在运行或暂停。默认是 "running"。                      |

- 使用

1. 编写动画

```css
@keyframes myfirst {
  0% {
    background: red;
    left: 0px;
    top: 0px;
  }
  25% {
    background: yellow;
    left: 200px;
    top: 0px;
  }
  50% {
    background: blue;
    left: 200px;
    top: 200px;
  }
  75% {
    background: green;
    left: 0px;
    top: 200px;
  }
  100% {
    background: red;
    left: 0px;
    top: 0px;
  }
}

@-webkit-keyframes myfirst /* Safari 与 Chrome */ {
  0% {
    background: red;
    left: 0px;
    top: 0px;
  }
  25% {
    background: yellow;
    left: 200px;
    top: 0px;
  }
  50% {
    background: blue;
    left: 200px;
    top: 200px;
  }
  75% {
    background: green;
    left: 0px;
    top: 200px;
  }
  100% {
    background: red;
    left: 0px;
    top: 0px;
  }
}
```

2. 在 div 上运行动画

```css
div {
  animation-name: myfirst;
  animation-duration: 5s;
  animation-timing-function: linear;
  animation-delay: 2s;
  animation-iteration-count: infinite;
  animation-direction: alternate;
  animation-play-state: running;
  /* Safari 与 Chrome: */
  -webkit-animation-name: myfirst;
  -webkit-animation-duration: 5s;
  -webkit-animation-timing-function: linear;
  -webkit-animation-delay: 2s;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-direction: alternate;
  -webkit-animation-play-state: running;
}
```

### 动画过渡与动画区别

1. 动画需要制作
2. 动画可以给多个元素使用
3. 动画可以倒放
4. 动画可以暂停
5. 动画不需要后期触发
6. 动画可以播放多次