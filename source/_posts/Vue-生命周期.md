---
title: Vue生命周期
date: 2019-5-23
tags: Vue
author: 映雪
---

浮生不若梦，孤影照惊鸿。

<!--more-->

![20190417151546517.jpg](/images/2022/01/11/lMLFZoGD23jAntw.jpg)

### 生命周期

- Vue 实例从开始创建、初始化数据、编译模板、渲染、更新 、准备销毁、销毁 等一系列过程就是生命周期。

- **Vue 的实例。它是 Vue 框架的入口。**

### 生命周期函数

- 对整个 Vue 实例生成、编译、挂载、销毁等过程进行控制。

### beforeCreate

- 在实例初始化之后,进行数据侦听和事件/侦听器的配置之前同步调用。

### created

- 在实例创建完成后被立即同步调用。在这一步中，实例已完成对选项的处理，意味着以下内容已被配置完毕：数据侦听、计算属性、方法、事件/侦听器的回调函数。然而，挂载阶段还没开始，且 $el property 目前尚不可用。

### beforeMount

- 在挂载开始之前被调用：相关的 render 函数首次被调用。该钩子在服务器端渲染期间不被调用。

### mounted

- 实例被挂载后调用，这时 el 被新创建的 vm.$el 替换了。如果根实例挂载到了一个文档内的元素上，当 mounted 被调用时 vm.$el 也在文档内。

- 注意: **mounted 不会保证所有的子组件也都被挂载完成**。如果你希望等到整个视图都渲染完毕再执行某些操作，可以在 mounted 内部使用 vm.$nextTick

```js
mounted: function () {
  this.$nextTick(function () {
    // 仅在整个视图都被渲染之后才会运行的代码
  })
}
```

- 该钩子在服务器端渲染期间不被调用。

### beforeUpdate

- 在数据发生改变后，DOM 被更新之前被调用。这里适合在现有 DOM 将要被更新之前访问它，比如移除手动添加的事件监听器。
- 该钩子在服务器端渲染期间不被调用，因为只有初次渲染会在服务器端进行。

### updated

- 在数据更改导致的虚拟 DOM 重新渲染和更新完毕之后被调用。

- 当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用计算属性或 watcher 取而代之。

- 注意，**updated 不会保证所有的子组件也都被重新渲染完毕**。如果你希望等到整个视图都渲染完毕，可以在 updated 里使用 vm.$nextTick

```js
updated: function () {
  this.$nextTick(function () {
    //  仅在整个视图都被重新渲染之后才会运行的代码
  })
}
```

- 该钩子在服务器端渲染期间不被调用。

### beforeDestroy

- 实例销毁之前调用。在这一步，实例仍然完全可用。

- 该钩子在服务器端渲染期间不被调用。

### destroyed

- 实例销毁后调用。该钩子被调用后，对应 Vue 实例的所有指令都被解绑，所有的事件监听器被移除，所有的子实例也都被销毁。

- 该钩子在服务器端渲染期间不被调用。

### activated

- keep-alive 主要用于保留组件状态或避免重新渲染。
- 被 keep-alive 缓存的组件激活时调用。
- 该钩子在服务器端渲染期间不被调用。

### deactivated

- 被 keep-alive 缓存的组件失活时调用。


### 父组件和子组件生命周期钩子函数执行顺序

1. 加载渲染过程

```js
父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted
```

2. 子组件更新过程

```js
父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated
```

3. 父组件更新过程

```js 
父 beforeUpdate -> 父 updated
```

4. 销毁过程

```js
父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed
```