---
title: JS兼容
date: 2019-4-20
tags: JS
author: 映雪
---

世上的爱情故事不是都有结局的。有些话只是说说而已，比如我爱你，比如我等你。

<!--more-->

### 兼容

- 不同浏览器支持的方法或属性有差异。

### 兼容问题


1. 事件对象的兼容问题（event）

- 想要获取鼠标坐标时候需用到event，在高级浏览器中会主动传递该参数，但是在IE8及以下浏览器中，将event放在了window.event属性下


```js
function fn(eve){
var e= event || window.event;
}
```

2. 获取键盘事件的键盘码的兼容问题（keyCode和IE：which）


```js
function fn(eve){
var e=event||window.event;
var e=e.keyCode||e.which;
}
```

3. 事件委托精确获取目标元素的兼容问题（target和IE：srcElement）

```js
function fn(eve){
var e=eve||window.event;
var target=e.target||e.srcElement;
}
```


4. 阻止事件的默认行为的兼容问题（preventDefault和IE：returnValue）

- 默认行为是浏览器之中非常重要的构成之一,比如标签类的超链接跳转、form表单提交、浏览器行为的右键菜单、鼠标按下文字选中等都属于浏览器的默认行为。


```js
function stopDefault(e){
if(e.preventDefault){
    e.preventDefault()
}else{
    e.returnValue = false;
}
}
```

5. 阻止事件冒泡的兼容问题（stopPropagation和IE：cancelBubble）

```js
function stopBubble(e){
if(e.stopPropagation){
    e.stopPropagation()
}else{
    e.cancelBubble = true;
}
}
```


6. 获取非行内样式（getComputedStyle和IE：currentStyle）

```js
function getStyle(ele,attr){
if(ele.currentStyle){
    return ele.currentStyle[attr];
}else{
    return getComputedStyle(ele,false)[attr];
}
}
```



7. 事件监听绑定兼容（绑定：addEventListener和IE：attachEvent）

```js
function addEvent(ele,type,callback){
if(ele.addEventListener){
    ele.addEventListener(type,callback,false)
}else if(ele.attachEvent){
    ele.attachEvent("on"+type,callback)
}else{
    ele["on"+type] = callback;
}
}
```

8. 事件监听移除兼容：removeEventListener和IE：detachchEvent）

```js
function removeEvent(ele,type,callback){
if(ele.removeEventListener){
    ele.removeEventListener(type,callback,false)
}else if(ele.detachEvent){
    ele.detachEvent("on"+type,callback)
}else{
    ele["on"+type] = null;
}
}
```

9. 获取滚动高度兼容（scrollTop）

```js
window.onscroll=function(){
    var scrollTop = document.documentElement.scrollTop || window.pageYOffset || document.body.scrollTop;
    console.log(scrollTop);
}
```