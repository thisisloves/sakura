---
title: Vuex-Module
date: 2019-09-01
tags: Vuex
author: 映雪
---

经一场大梦，梦中见满眼山花如翡，如见故人，喜不自胜。

<!--more-->

### Module

- 由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。
- Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter。

### 分割模块

- 新建 modules 文件夹，创建子模块

![截屏2022-04-07 下午2.49.50.png](/images/2022/04/07/6WwTJqea8UBSKjg.png)

### 子模块

- namespaced 设置为 true

```js
export default {
  namespaced: true,
  state: {
    name: 'home小明',
    age: 24,
    like: 'wegame',
  },
  getters: {
    getNameLengthHome: (state) => (value) => {
      return state.name.length + value;
    },
  },
  actions: {
    changeNameHomeAction(context, payload) {
      setTimeout(() => {
        context.commit({ type: 'changeNameHome', name: payload.name });
      }, 1000);
    },
  },
  mutations: {
    changeNameHome(state, payload) {
      state.name = payload.name;
    },
    changeAgeHome(state, payload) {
      state.age = payload.age;
    },
  },
};
```

- modules 下的 index.js 是模块出口总和

```js
import login from './login';
import home from './home';

export default {
  home,
  login,
};
```

### 最上级模块

- 将模块引入

```js
import { createStore } from 'vuex';
import modules from './modules/index';

export default createStore({
  state: {
    name: '222',
  },
  mutations: {
    changeName(state, payload) {
      state.name = payload.name;
    },
  },
  getters: {
    getNameLength: (state) => (value) => {
      return state.name.length + value;
    },
  },
  actions: {
    changeNameAction(context, payload) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          if (payload.name.length > 4) {
            context.commit({ type: 'changeName', name: payload.name });
            resolve('dispatch异步成功');
          } else {
            reject('dispatch异步失败');
          }
        }, 1000);
      });
    },
  },
  modules: { ...modules },
});
```

### 在组件中使用

- 获取子模块 state

```jsx
   <a>模块home的name: {{this.$store.state.home.name}}</a>
   <a>模块home的age:{{this.$store.state.home.age}}</a>
   <a>模块home的active:{{this.$store.state.home.active}}</a>
```

- 调用子模块 modules 的方法

```js
this.$store.commit({ type: 'home/changeAgeHome', age: 'home-commit-change' });
```

- 调用子模块 actions 的方法

```js
this.$store.dispatch({ type: 'home/changeNameHomeAction', name: 'home-actions-change' });
```

- 调用子模块 getters 的方法

```js
this.$store.getters['home/getNameLengthHome'](12);
```

### 辅助函数

```jsx
<template>
  <a>模块login的name:{{ nameLogin }}</a>
  <a>模块login的age:{{ ageLogin }}</a>
  <a>模块login的active:{{ activeLogin }}</a>
  <a>模块login的name的length:{{ getNameLengthLogin(12) }}</a>
</template>
<script>
import { mapMutations, mapState, mapActions, mapGetters } from 'vuex';
export default {
  data() {
    return {
    };
  },
  computed: {
    ...mapState({
      nameLogin: (state) => state.login.name,
      ageLogin: (state) => state.login.age,
      activeLogin: (state) => state.login.like,
    }),
    ...mapGetters('login',['getNameLengthLogin']),
  },

  methods: {
    ...mapMutations('login', ['changeAgeLogin']),
    ...mapActions('login', ['changeNameLoginAction']),
    changeLoginAge() {
      this.changeAgeLogin({age:'change-age-login'})
    },
    changeLoginName() {
      this.changeNameLoginAction({name:'change-name-login'})
    },
  },
};
</script>
```
