---
title: CSS总结
date: 2018-4-14
tags: CSS
author: 映雪
---

有种花，超出三界之外，不在五行之中，生于弱水彼岸，无茎无叶，绚烂绯红。佛说：那是引魂之花——彼岸花。花开一千年，叶一千年，花叶永不相见。彼岸花开只一团火红，相念相惜却不得相见，独自彼岸路。

<!--more-->

### 布局

1. 静态布局
2. 响应式布局
3. 流式布局
4. 悬挂式布局
5. 百分比布局
6. 网格布局
7. 双飞布局
8. 圣杯布局
9. 输入框布局
10. 骰子布局


### 双飞布局

```html
<style>
    *{margin:0;padding:0;}
    html，body{height:100%;width:100%;}
    .left{width:200px;background:red;height:100%;position:absolute;top:0;bottom:0}
    .center{margin:0 200px;background:yellow;height:100%;}
    .right{width:200px;background:blue;height:100%;position:absolute;right:0;top:0;bottom:0;}
</style>
</head>
<body>
    <div class="left">123</div>
    <div class="center">2323</div>
    <div class="right">123</div>
</body>
```

### 单飞布局

```html
<style>
    *{margin:0;padding:0;}
    html，body{height:100%;width:100%;}
    .left{width:200px;background:red;height:100%;float:left;}
    .center{margin:0 0 0 200px;background:yellow;height:100%;}
</style>
</head>
<body>
    <div class="left">123</div>
    <div class="center">2323</div>
</body>
```

### 圣杯布局

```html
<style>
    *{margin:0;padding:0;}
    html，body{height:100%;width:100%;}
    .top{height:100px;width:100%; background:blue;}
    .center{height:100%;width:100%;overflow:hidden;}
    .center .left{width:100px;background:yellow;height:100%;float:left;}
    .center .right{width:100px;background:black;height:100%;float:right;}
    .center .content{height:100%;margin:0 100px;background:red;width:100%;}
    .bottom{height:100px;width:100%;background:red;}
</style>
</head>
<body>
    <div class="top">top</div>

    <div class="center">
    <div class="left">left</div>
    <div class="right">right</div>
    <div class="content">content</div>
    </div>

    <div class="bottom">bottom</div>
</body>
```

### 五大居中

```html
<style>
    *{padding:0;margin:0;}

    /* .box{width:400px;height:400px;border:1px solid  black;text-align:center;position:fixed;left:0;right:0;top:0;bottom:0;margin:auto;}/*div居中，记得加 margin:auto; 居中2*/

    /* .box{width:400px;height:400px;border:1px solid black;text-align:center;position:fixed;left:50%;top:50%;margin:-204px 0 0 -204px} 居中3*/

    /*.box{width:400px;height:400px;border:1px solid black;text-align:center;position:fixed;left:50%;top:50%;transform:translate(-50% ,-50%);}  居中4*/

    .box{width:400px;height:400px;border:1px solid black;text-align:center;display:flex;justify-content:center;align-items:center;}/*第5居中，居中图片*/

    .box p{display:inline-block;background:red;vertical-align:middle;height:100px;}

    .box:after{content:"";display:inline-block;height:100%;vertical-align:middle;} /*图片居中* 居中1*/
</style>
<body>
    <div class="box">
    <p>11111111111111111</p>
    </div>
</body>
```

### 边框三角

```html
<style>
    .box{border-top:10px solid red;border-bottom:10px solid blue;border-left:10px yellow solid;border-right:10px solid green;width:0;margin:50px auto;}
    .box-2{border-bottom:10px solid blue;border-left:10px white solid;border-right:10px solid white;width:0;margin:50px auto;}
</style>
</head>
<body>
    <div class="box"></div>
    <div class="box-2"></div>
</body>
```

### CSS兼容内核

- 谷歌 Blink 兼容前缀
- 火狐 Gecko -moz-
- ie Trident -ms-
- 苹果 webkit -webkit-
- opera persto -o-

### 高度塌陷解决

1. 给父元素书写 overflow:hidden;
2. 给父元素书写 display:table;
3. 伪类元素清除浮动:after{content:"";display:block;clear:both;visibility:hidden;}
4. 通过空白撑开



### BFC 触发条件

1. overflow:hidden;
2. float:
3. position:absolute
4. display:flex
5. position:fixed
6. display:inlien-block


### CSS 可以被继承的属性（多为字体）

1. color
2. font
3. font-size
4. font-weight
5. font-family
6. font-style
7. line-height
8. text-decoration
9. text-indent
10. text-align
11. word-warp
12. text-shadow
13. text-transform
14. list-style


### 单行省略号

```
white-space:nowrap; overflow:hidden;text-overflow：ellipsis;width:200px;
```

### 透明

```css
opactity:0-1;
filter:alpha(opactity:0-100;) 非ie无效
```

### 浏览器智能表单默认清除

```js
nobalidate="false"
autocomplete="off"   //火狐
```

### 滚动条隐藏

```css
谷歌和火狐
.element::-webkit-scrollbar { width: 0 !important }

IE 10+
.element { -ms-overflow-style: none; }

Firefox
.element { overflow: -moz-scrollbars-none; }
```

### 图片整合（精灵图）

- 将小图标，按钮背景图片等有规则的合并成一张背景图，即将多张图片合为一张整图，然后用 background-position 来实现背景图片的定位技术。
- 优劣势：
1. 通过图片整合来减少对服务器的请求次数，从而提高 页面的加载速度。
2. 通过整合图片来减小图片的体积。
3. 增加了开发人员的负担。