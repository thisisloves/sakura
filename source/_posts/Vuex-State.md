---
title: Vuex-State
date: 2019-4-1
tags: Vuex
author: 映雪
---

千灯万盏，我只想去有你的那户人家。

<!--more-->

### State

- Vuex 用一个对象就包含了全部的应用层级状态，作为一个“唯一数据源“ (SSOT) 而存在。
- 单一状态树让我们能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照。

```js
import { createStore } from 'vuex';

export default createStore({
  state: {
    name: '222',
  },
  mutations: {
    fixName(state, payload) {
      state.name = payload.name;
    },
  },
  actions: {},
  modules: {},
});
```

### State 获取

- 由于 Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在计算属性中返回某个状态。

```html
<template>
  <a>这是vuex的state内容：{{ name }}</a>
</template>
<script>
  export default {
    computed: {
      name() {
        return this.$store.state.name;
      },
    },
  };
</script>
```

- state 变化的时候, 都会重新求取计算属性，并且触发更新相关联的 DOM。

### mapState 辅助函数

- 当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。
- 为了解决这个问题，我们可以使用 mapState 辅助函数帮助我们生成计算属性。

```html
<template>
  <a>这是vuex的state内容：{{ name }}</a>
  <a>这是vuex的state内容：{{ nameAlias }}</a>
  <a>这是vuex的state内容：{{ getStateName }}</a>
</template>

<script>
  import { mapState } from 'vuex';
  export default {
    data() {},
    computed: mapState({
      // 代码简练
      name: (state) => state.name,
      // 等同上面
      nameAlias: 'name',
      // 可使用 this
      getStateName(state) {
        return state.name;
      },
    }),
  };
</script>
```

- 当映射的计算属性的名称与 state 的子节点名称相同时，可以给 mapState 传一个字符串数组。

```js
computed: mapState([
  // 映射 this.name 为 store.state.name
  'name'
])
```

### 计算属性混用

- 通过扩展运算符可以和局部计算属性混合使用。

```js
  computed: {
    ...mapState(['name']),
  },
```
