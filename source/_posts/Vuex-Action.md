---
title: Vuex-Action
date: 2019-8-15
tags: Vuex
author: 映雪
---

留人间多少爱，迎浮世千重变，和有情人做快乐事，不问是劫是缘。

<!--more-->

### Action

- Action 类似于 mutation

- 不同之处

1. Action 提交的是 mutation，而不是直接变更状态。
2. Action 可以包含任意异步操作。

### Action 使用

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
  getters: {
    getNameLength: (state) => (value) => {
      return state.name.length + value;
    },
  },
  actions: {
    changeName(context, payload) {
      setTimeout(() => {
        context.commit({ type: 'changeName', name: payload.name });
      }, 1000);
    },
  },
  modules: {},
});
```

- 组件调用

```js
this.$store.dispatch({ type: 'changeName', name: value });
```

### Action 分发

- mutation 必须同步执行，Action 不受约束

```js
actions: {
  checkout ({ commit, state }, products) {
    // 把当前购物车的物品备份起来
    const savedCartItems = [...state.cart.added]
    // 发出结账请求，然后乐观地清空购物车
    commit(types.CHECKOUT_REQUEST)
    // 购物 API 接受一个成功回调和一个失败回调
    shop.buyProducts(
      products,
      // 成功操作
      () => commit(types.CHECKOUT_SUCCESS),
      // 失败操作
      () => commit(types.CHECKOUT_FAILURE, savedCartItems)
    )
  }
}
```

### mapActions 辅助函数

- mapActions 辅助函数将 store 中的 actions 方法映射到组件
- this.$store.dispatch() 的另一种写法

```js
import { mapActions } from 'vuex';

export default {
  // ...
  methods: {
    // 将 `this.changeName()` 映射为 `this.$store.dispatch('changeName')`
    ...mapActions(['changeName']),

    // 将 `this.changeNameBy(name)` 映射为 `this.$store.dispatch('changeNameBy', name)`
    ...mapActions(['changeName', 'changeNameBy']),

    // 将 `this.changeNameFn()` 映射为 `this.$store.dispatch('changeName')`
    ...mapActions({
      changeNameFn: 'changeName',
    }),

    fixName() {
      this.changeName({ name: value });
    },
  },
};
```

### 异步 Action

- store.dispatch 可以处理被触发的 action 的处理函数返回的 Promise，并且 store.dispatch 仍旧返回 Promise 。

```js
 actions: {
    changeName(context, payload) {
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
  }
```

- 调用

```js
this.$store.dispatch({ type: 'changeName', name: value }).then((response) => {
  console.log(response);
});
```


### 组合 Action


```js
// 假设 getData() 和 getOtherData() 返回的是 Promise

actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // 等待 actionA 完成
    commit('gotOtherData', await getOtherData())
  }
}
```

- 一个 store.dispatch 在不同模块中可以触发多个 action 函数。在这种情况下，只有当所有触发函数完成后，返回的 Promise 才会执行。