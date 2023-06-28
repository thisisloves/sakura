---
title: Redux-saga
date: 2019-11-10
tags: Redux
author: 映雪
---

他只字未提我爱你，你却句句都是我愿意，他不过是暧昧成瘾，而你却轻易走了心。

<!--more-->

### Redux 数据流

![截屏2022-04-25 下午3.13.16.png](/images/2022/04/25/16qCNxnT3AOjHiQ.png)

### Redux-Middleware 中间件

1. 组件中的异步操作

- 更新 state 通常直接通过同步的操作来 dispatch action，state 就会被立即更新，但是真实开发中，redux 中保存的很多数据可能来自服务器，我们需要进行异步的请求，再将数据保存到 redux 中。

![截屏2022-04-25 下午3.25.28.png](/images/2022/04/25/DPmY8FaEcgK2hqz.png)

- 缺陷：要把所有网络请求的异步代码放到组件的生命周期中来完成。

2. redux 中的异步操作

- 网络请求到的数据也属于状态管理的一部分，更好的一种方式应该是将其也交给 redux 来管。

![截屏2022-04-25 下午3.25.50.png](/images/2022/04/25/PrIhTkn5AMauC2N.png)

- 缺陷：在 Redux 中 reducer 应该为纯函数，只承担计算 State 的功能，这时候就需要中间件转化支持。

### 中间件原理

- dispatch 一个 action 之后，到达 reducer 之前，进行一些额外的操作，就需要用到 middleware。可以利用 Redux middleware 来进行日志记录、创建崩溃报告、调用异步接口或者路由等等。换言之，中间件都是对 store.dispatch()的增强。

![截屏2022-04-25 下午3.39.20.png](/images/2022/04/25/Kb7g2rn63y1R5cs.png)

### Redux-saga 流程

![截屏2022-04-28 上午10.15.30.png](/images/2022/04/28/Ie2KSm9xbZgwvTD.png)

- redux-saga是另一个比较常用在redux发送异步请求的中间件,它的使用比react-thunk更加的灵活。


### Effect 

- 一个 effect 就是一个 Plain Object JavaScript 对象，包含一些将被 saga middleware 执行的指令。

1. take(pattern)

- take函数可以理解为监听未来的action，它创建了一个命令对象，告诉middleware等待一个特定的action，Generator会暂停，直到一个与pattern匹配的action被发起，才会继续执行下面的语句，也就是说，take是一个阻塞的 effect。

```js
function* watchFetchData() {
 while(true) {
 // 监听一个type为 'FETCH_REQUESTED' 的action的执行，直到等到这个Action被触发，才会接着执行下面的 yield fork(fetchData) 语句
  yield take('FETCH_REQUESTED');
  yield fork(fetchData);
 }
}
```

2. put(action)
put函数是用来发送action的 effect，可以简单的把它理解成为redux框架中的dispatch函数，当put一个action后，reducer中就会计算新的state并返回，put 也是阻塞 effect。

```js
export function* toggleItemFlow() {
 let list = []
 // 发送一个type为 'UPDATE_DATA' 的Action，用来更新数据，参数为 `data：list`
 yield put({
  type: actionTypes.UPDATE_DATA,
  data: list
 })
}
```

3. call(fn, ...args)

- call函数简单的理解为可以调用其他函数的函数，它命令 middleware 来调用fn 函数，args为函数的参数，注意：fn 函数可以是一个 Generator 函数，也可以是一个返回 Promise 的普通函数，call 函数也是阻塞 effect。

```js
export const delay = ms => new Promise(resolve => setTimeout(resolve, ms))
export function* removeItem() {
 try {
 // 这里call 函数就调用了 delay 函数，delay 函数为一个返回promise 的函数
 return yield call(delay, 500)
 } catch (err) {
 yield put({type: actionTypes.ERROR})
 }
}
```

4. fork(fn, ...args)

- fork 函数和 call 函数很像，都是用来调用其他函数的，但是fork函数是非阻塞函数，也就是说，程序执行完 yield fork(fn， args) 这一行代码后，会立即接着执行下一行代码语句，而不会等待fn函数返回结果后再执行下面的语句。

```js
import { fork } from 'redux-saga/effects'
export default function* rootSaga() {
 // 下面的四个 Generator 函数会一次执行，不会阻塞执行
 yield fork(addItemFlow)
 yield fork(removeItemFlow)
 yield fork(toggleItemFlow)
 yield fork(modifyItem)
}
```

5. select(selector, ...args)

- select 函数是用来指示 middleware调用提供的选择器获取Store上的state数据，可以简单的把它理解为redux框架中获取store上的 state数据一样的功能 ：store.getState()。

```js
export function* toggleItemFlow() {
  // 通过 select effect 来获取 全局 state上的 `getTodoList` 中的 list
  let tempList = yield select(state => state.getTodoList.list)
}
```

### 辅助函数

1. takeEvery

- takeEvery，同一个action多次触发，每个都会执行，，takeEvery 允许多个 fetchData 实例同时启动

```js
import { takeEvery } from 'redux-saga'
function* watchFetchData() {
 yield* takeEvery("FETCH_REQUESTED", fetchData)
}
```

2. takeLatest

- 多个 fetchData 尚未结束，只想得到最新那个请求的响应可以使用 takeLatest，任何时刻 takeLatest 只允许执行一个 fetchData 任务，并且这个任务是最后被启动的那个，如果之前已经有一个任务在执行，那之前的这个任务会自动被取消。

```js
import { takeLatest } from 'redux-saga'
function* watchFetchData() {
 yield* takeLatest('FETCH_REQUESTED', fetchData)
}
```
