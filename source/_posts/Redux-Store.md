---
title: Store
date: 2019-4-24
tags: Redux
author: 映雪
---

他在这里是自由的，随便享受风、天光和春去秋来这个城市不同的气味，有时候是槐花，有时候是树叶，有时候是下面街上卖菠萝的甜香。

<!--more-->


### Store

- Store 是把 Action 和 Reducer 联系到一起的对象。
- Redux 应用只有一个单一的 store。


### Store 职责

1. 维持应用的 state
2. 提供 getState() 方法获取 state
3. 提供 dispatch(action) 方法更新 state
4. 通过 subscribe(listener) 注册监听器
5. 通过 subscribe(listener) 返回的函数注销监听器

### 创建 Store

```js
import reduxClass from './reducer/class';
import reduxUser from './reducer/user';

const reducer = combineReducers({
  reduxClass,
  reduxUser,
});
const store = createStore(reducer);

export default store;
```

- createStore() 的第二个参数是可选的, 用于设置 state 初始状态。这对开发同构应用时非常有用，服务器端 redux 应用的 state 结构可以与客户端保持一致, 那么客户端可以将从网络接收到的服务端 state 直接用于本地数据初始化。

```js
let store = createStore(reducer, window.STATE_FROM_SERVER)
```