---
title: Vuex-Mutation
date: 2019-5-12
tags: Vuex
author: 映雪
---

清欢不渡，白茶不予。

<!--more-->

### Mutation

- 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。
- 每个 mutation 都有一个字符串的事件类型 (type)和一个回调函数 (handler) 。

### Mutation 使用

```js
import { createStore } from 'vuex';

export default createStore({
  state: {
    name: '222',
  },
  mutations: {
    changeName(state, payload) {
      state.name = payload.name;
    },
  },
  actions: {},
  modules: {},
});
```

### 提交负荷 (payload)

- store.commit 传入额外的参数，即 mutation 的载荷（payload）
- 在大多数情况下，载荷应该是一个对象，可以包含多个字段并且记录的 mutation 会更易读。

```js
this.$store.commit('changeName', {
  name: '小明',
});
```

- 对象风格提交

```js
store.commit({
  type: 'changeName',
  name: '小明',
});
```

### mapMutations 辅助函数

- this.$store.commit() 的另一种写法。
- mapMutations 辅助函数将组件中的 methods 映射为 store.commit 调用。

```js
import { mapMutations } from 'vuex';

export default {
  // ...
  methods: {
    // 将 `this.changeName()` 映射为 `this.$store.commit('changeName')`
    ...mapMutations(['changeName']),

    // 将 `this.changeNameBy(name)` 映射为 `this.$store.commit('changeNameBy', name)`
    ...mapMutations(['changeName', 'changeNameBy']),

    // 将 `this.changeNameFn()` 映射为 `this.$store.commit('changeName')`
    ...mapMutations({
      changeNameFn: 'changeName',
    }),

    fixName() {
      this.changeName({ name: value });
    },
  },
};
```

### 注意

- Mutation 必须是同步函数，当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用。
