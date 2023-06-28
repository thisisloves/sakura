---
title: Vuex基础
date: 2019-3-31
tags: Vuex
author: 映雪
---

凛冬散尽，星河长明。

<!--more-->

### Vuex

- 专为 Vue.js 应用程序开发的状态管理模式，集中存储和管理应用的所有组件状态。

### Vuex 理解

- 状态：数据，相当于组件内部的 data 的返回值，Vue 是数据驱动的，数据变化往往会表现在视图层。
- 集中存储：Vue 只关注视图层，Vuex 提供了一个仓库（store）来保存数据，使得数据和视图分离。
- 管理：处理保存数据，还可计算、处理数据。
- 所有组件状态：所有组件都可获取仓库中的数据，即一个项目只有一个数据源。

![截屏2022-04-02 下午3.40.39.png](/images/2022/04/02/VSoesTE8qBuYO6p.png)

### Vuex 基础使用

1. 创建 store

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

2. 在 main.js 引入

```js
import { createApp } from 'vue';
import ElementPlus from 'element-plus';
import 'element-plus/dist/index.css';
import App from './App.vue';
import router from './router';
import store from './store';

createApp(App).use(store).use(router).use(ElementPlus).mount('#app');
```

3. 组件内使用

```html
<template>
  <el-input @change="fixValue" v-model="inputData" />
  这是vuex的name内容：{{ this.$store.state.name }}
</template>

<script>
  export default {
    data() {
      return {
        inputData: this.$store.state.name,
      };
    },
    methods: {
      fixValue(value) {
        this.$store.commit({ type: 'changeName', name: value });
      },
    },
  };
</script>
```

### Vuex 持久化 

- 当刷新页面,项目重新加载,vuex 会重置,所有状态回到初始状态,使用 vuex-persistedstate 可以避免这种情况。

1. 安装

```
npm install --save vuex-persistedstate
```

2. 使用

```js
import Vuex from "vuex";
import createPersistedState from "vuex-persistedstate";
const store = new Vuex.Store({
  plugins: [createPersistedState()],
});
```
