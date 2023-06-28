---
title: Vue引用
date: 2019-6-22
tags: Vue
author: 映雪
---

知白守黑，和光同尘。

<!--more-->

### vm.$refs

- 一个对象，持有已注册过 ref 的所有组件。

### ref

- 用来给元素或者子组件注册引用信息。引用信息将会注册在父组件的$refs 对象上。如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素，如果用在子组件上，引用就指向组件实例。
- 当 v-for 用于元素或者组件的时候，引用信息将是包含 DOM 节点或组件实例的数组。

```html
<p ref="p">hello</p>
<child-comp ref="child"></child-comp>
```

### ref 注册

1. ref 本身是作为渲染结果被创建的，在初始渲染的时候你不能访问它们 它们还不存在。
2. $refs 也不是响应式的，只在组件渲染完成后才填充，因此不应该用它在模板中做数据绑定。

```html
<div id="app">
     <input type="text" ref="input1" id="input1"/>
     <button @click="add">添加</button>
</div>
<script src="../js/vue.js"></script>
<script type="text/javascript">
  var Vuepro=new Vue({
    el:'#app',
    methods:{
      add:function() {
        this.$refs.input1.value="22";   //this.$refs.input1 减少获取dom节点的消耗
        console.log(this.$refs.input1) //<input type="text"id="input1">
        console.log(document.getELementById('input1')) //<input type="text" id="input">
      }
    }
  })
</script>
```

- refs 相对 document.getElementById 的方法，会减少获得 dom 节点的消耗。

