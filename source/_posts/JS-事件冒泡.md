---
title: JS事件冒泡
date: 2019-4-5
tags: JS
author: 映雪
---

山下梅子酒，十里故清欢。

<!--more-->

### 事件

- 事件是文档和浏览器窗口中发生的特定的交互瞬间。 事件是javascript应用跳动的心脏，也是把所有东西黏在一起的胶水，当我们与浏览器中web页面进行某些类型的交互时，事件就发生了。
- 事件可能是用户在某些内容上的点击，鼠标经过某个特定元素或按下键盘上的某些按键，事件还可能是web浏览器中发生的事情，比如说某个web页面加载完成，或者是用户滚动窗口或改变窗口大小。

 ### 事件流

- 事件流描述的是从页面中接受事件的顺序，但有意思的是，微软（IE）和网景（Netscape）开发团队居然提出了两个截然相反的事件流概念，IE的事件流是事件冒泡流(event bubbling)，而Netscape的事件流是事件捕获流(event capturing)。

1. 事件捕获阶段：当某个元素触发某个事件时，首先会触发根元素document，然后事件将沿着dom树向下传播给目标节点的每一个祖先节点，直到目标节点。
2. 目标阶段：到达目标元素之后，执行目标元素的事件处理函数
3. 事件冒泡阶段：从目标元素开时，事件将沿着DOM树向上传播，直到根节点。

![324770-20160630170126421-1509815715.jpeg](/images/2022/03/24/KS1Uxz438WasXk7.jpg)

- 之所以会有冒泡和捕获事件(IE 9 之前的浏览器不支持捕获事件)，毕竟在实际中处理事情肯定有个先后顺序，要么由里向外，要么由外向里，两者都是必须的。

### 事件冒泡

- 冒泡事件，由里向外，最里层的元素先执行，然后冒泡到外层。

```html
<body>
    <div id ="div1" style="width:400px;height:400px;background: red;">
    div1
        <div id ="div2" style="width:300px;height:300px;background: blue;">
        div2
            <div id ="div3" style="width:200px;height:200px;background: green;">
            div3
            </div>
        </div>
    </div>
</body>
<script>

const div1 = document.getElementById('div1');
const div2 = document.getElementById('div2');
const div3 = document.getElementById('div3');

// 事件冒泡
div1.onclick = function(){
    console.log('我是div1')
}
div2.onclick = function(){
    console.log('我是div2')
}
div3.onclick = function(){
    console.log('我是div3')
}

// addEventListener 写法
div1.addEventListener('click',function(){
    console.log('我是div1')
},false)

div2.addEventListener('click',function(){
    console.log('我是div2')
},false)

div3.addEventListener('click',function(){
    console.log('我是div3')
},false)
<script>
```

- 当点击最上层的div3，会先触发div3事件，然后一层层冒泡外层，div3 到 div2 最后到 div1 

### 捕获事件

- 由外向里，最外层的元素先执行，然后传递到内部。

```html
<body>
    <div id ="div1" style="width:400px;height:400px;background: red;">
    div1
        <div id ="div2" style="width:300px;height:300px;background: blue;">
        div2
            <div id ="div3" style="width:200px;height:200px;background: green;">
            div3
            </div>
        </div>
    </div>
</body>
<script>

const div1 = document.getElementById('div1');
const div2 = document.getElementById('div2');
const div3 = document.getElementById('div3');

// 事件捕获
div1.addEventListener('click',function(){
    console.log('我是div1')
},true)

div2.addEventListener('click',function(){
    console.log('我是div2')
},true)

div3.addEventListener('click',function(){
    console.log('我是div3')
},true)
<script>
```

- 当点击最上层的div3，会先触发div1事件，然后一层层往里层处罚，div1 到 div2 最后到 div3 


### 停止冒泡

- 只执行最内层或最外层的事件，根据内外层关系来把范围更广的事件取消掉。
- event.stopPropagation()(IE 中 window.event.cancelBubble = true)可以用来取消事件冒泡。

```js
div1.onclick = function(e){
    e.stopPropagation()
    console.log('我是div1')
}
div2.onclick = function(e){
    e.stopPropagation()
    console.log('我是div2')
}
div3.onclick = function(e){
    e.stopPropagation()
    console.log('我是div3')
}
```

- 兼容写法

```js
function stopBubble(e) {
//如果提供了事件对象，则这是一个非IE浏览器
if (e.stopPropagation)
    //因此它支持W3C的stopPropagation()方法
    e.stopPropagation();
else
    //否则，我们需要使用IE的方式来取消事件冒泡
    e.cancelBubble = true;
}
```

- 结果是：当点击 div3 或 div2 的时候，后就不会在向下执行了。


注意：addEventListener()必须用removeEventListener()解除