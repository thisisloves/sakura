---
title: Reducer
date: 2019-3-10
tags: Redux
author: 映雪
---

月遇从云，花遇和风，今晚上的夜空很美，我又想你了。

<!--more-->

### Reducer

- Reducers 指定了应用状态的变化如何响应 actions 并发送到 store 的， actions 只是描述了有事情发生了这一事实，并没有描述应用如何更新 state。

### State 结构

- 在 Redux 应用中，所有的 state 都被保存在一个单一对象中。
- 开发复杂的应用时，不可避免会有一些数据相互引用。建议你尽可能地把 state 范式化，不存在嵌套。把所有数据放到一个对象里，每个数据以 ID 为主键，不同实体或列表间通过 ID 相互引用数据。把应用的 state 想像成数据库。

### 编写 Reducer

- reducer 就是一个纯函数，接收旧的 state 和 action，返回新的 state。
- 保持 reducer 纯净非常重要。

1. 不要修改传入参数
2. 不要执行有副作用的操作，如 API 请求和路由跳转
3. 不要调用非纯函数，如 Date.now() 或 Math.random()

```js
const init = {
  number: 0
}
//eslint-disable-next-line
export default (state = init, action) => {
  switch (action.type) {
      case 'addCount':
          return {...state,number :state.number + action.count}
      case 'reduceCount':
          return {...state,number :state.number - action.count}
      default:
          return state
  }
}
```

- 不直接修改 state 中的字段，而是返回新对象。