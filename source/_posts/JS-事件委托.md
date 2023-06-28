---
title: JS事件委托
date: 2019-4-3
tags: JS
author: 映雪
---

故里有长安，长安归故里。

<!--more-->


### 事件委托

- 把原本需要绑定在子元素的响应事件（click、keydown......）委托给父元素，让父元素担当事件监听的职务。
- 事件委托的原理是DOM元素的事件冒泡。

### 事件流

- 事件触发后，会在子元素和父元素之间传播（propagation）。这种传播分成三个阶段

1. 捕获阶段：从window对象传导到目标节点（上层传到底层）称为“捕获阶段”（capture phase），捕获阶段不会响应任何事件；
2. 目标阶段：在目标节点上触发，称为“目标阶段”
3. 冒泡阶段：从目标节点传导回window对象（从底层传回上层），称为“冒泡阶段”（bubbling phase）。事件代理即是利用事件冒泡的机制把里层所需要响应的事件绑定到外层


### 事件监听

- 给DOM元素添加一个事件监听，只能收到对应事件类型的消息，收到这个消息时，执行事件回调函数

```js
obox.addEventListener("click",callback,false)  //正常浏览器
obox.attachEvent("onclick",callback)  //IE

obox.removeEventListener("click",fn1);
obox.detachEvent("onclick",fn1)
```

### 事件委托实现

- 利用事件冒泡原理，将绑定在多个子元素的相同事件，绑定在页面上现存的父元素身上。

```js
oul.onclick=function(eve){
    var e=eve||window.event;
    var target=e.target||e.srcElement;
    if(target.nodeName=="LI"){
    console.log(target.innerHTML);
    }
}
```

### 事件委托的优点
 
1. 减少内存消耗

- 若果我们有一个列表，列表之中有大量的列表项，我们需要在点击列表项的时候响应一个事件

```html
<ul id="list">
<li>item 1</li>
<li>item 2</li>
<li>item 3</li>
......  //未知
<li>item n</li>
</ul>
```

- 如果给每个列表项一一都绑定一个函数，那对于内存消耗是非常大的，效率上需要消耗很多性能，因此，比较好的方法就是把这个点击事件绑定到他的父层，也就是 `ul` 上，然后在执行事件的时候再去匹配判断目标元素，所以事件委托可以减少大量的内存消耗，节约效率。

2. 动态绑定事件
- 事件是绑定在父层的，和目标元素的增减是没有关系的，执行到目标元素是在真正响应执行事件函数的过程中去匹配的，将多个相同子元素的相同事件委托给页面上现存的共同的父元素，利用了事件冒泡的原理，配合事件源找到真正点击的元素，做事件处理。

3. javaScript和DOM节点之间的关联变少了，这样也就减少了因循环引用而带来的内存泄漏发生的概率。

