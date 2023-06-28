---
title: Vue页面缓存
date: 2019-4-22
tags: Vue
author: 映雪
---

低头暗衬金杉树,仰视欲接红绣球。

<!--more-->

### keep-alive

- keep-alive 是 Vue 的内置组件，当它包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。
- 和 transition 相似，keep-alive 是一个抽象组件，它自身不会渲染成一个 DOM 元素，也不会出现在父组件链中。

### keep-alive 功能

1. 组件之间切换，保持这些组件的状态，以避免反复重新渲染导致的性能问题。
2. 可以很大程度上减少接口请求，减小服务器压力。

### Props

1. include - 字符串或正则表达式。只有名称匹配的组件会被缓存。
2. exclude - 字符串或正则表达式。任何名称匹配的组件都不会被缓存。
3. max - 数字。最多可以缓存多少组件实例。

### keep-alive 原理

- 在 created 函数调用时将需要缓存的 VNode 节点保存在 this.cache 中／在 render（页面渲染） 时，如果 VNode 的 name 符合缓存条件（可以用 include 以及 exclude 控制），则会从 this.cache 中取出之前缓存的 VNode 实例进行渲染。

### 生命周期函数

- 被包含在 keep-alive 中创建的组件，会多出两个生命周期的钩子: activated 与 deactivated。

1. activated

- 在 keep-alive 组件激活时调用，该钩子函数在服务器端渲染期间不被调用

2. deactivated

- 在 keep-alive 组件停用时调用，该钩子在服务器端渲染期间不被调用
- 使用 keep-alive 会将数据保留在内存中，如果要在每次进入页面的时候获取最新的数据，需要在 activated 阶段获取数据，承担原来 created 钩子函数中获取数据的任务。

- 注意

1. 只有组件被 keep-alive 包裹时，这两个生命周期函数才会被调用，如果作为正常组件使用，是不会被调用的，以及在 2.1.0 版本之后，使用 exclude 排除之后，就算被包裹在 keep-alive 中，这两个钩子函数依然不会被调用！另外，在服务端渲染时，此钩子函数也不会被调用。
2. 使用了 keep-alive 就不会调用 beforeDestroy(组件销毁前钩子)和 destroyed(组件销毁)，因为组件没被销毁，被缓存起来了。

### keep-alive 使用

- app.js

```html
<template>
  <router-view v-slot="{ Component }">
    <keep-alive :max="10" :include="includeList">
      <component :is="Component" />
    </keep-alive>
  </router-view>
</template>
<script>
export default {
  data() {
    return {
      includeList: ['Computed','KeepLive'],
      excludeList:['Watch']
    };
  },
</script>
```


- 当切换出Computed 组件再切换回来，计算的结果值不会因为切换路由而初始化。       

![截屏2022-04-08 下午4.32.28.png](/images/2022/04/08/sevzrauSpWE1nKg.png)