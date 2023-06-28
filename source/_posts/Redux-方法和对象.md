---
title: Redux的方法和对象
date: 2019-3-10
tags: Redux
author: 映雪
---

愿衣襟带花，岁月风平，深情皆不负。

<!--more-->

### Redux 顶级暴露的方法

(1). createStore(reducer, [preloadedState], [enhancer])

- 创建一个 Redux store 来以存放应用中所有的 state。

* 参数

1. reducer (Function): 接收两个参数，分别是当前的 state 树和要处理的 action，返回新的 state 树。
2. [preloadedState] (any): 初始时的 state。 在同构应用中，你可以决定是否把服务端传来的 state 水合（hydrate）后传给它，或者从之前保存的用户会话中恢复一个传给它。如果你使用 combineReducers 创建 reducer，它必须是一个普通对象，与传入的 keys 保持同样的结构。否则，你可以自由传入任何 reducer 可理解的内容。
3. enhancer (Function): Store enhancer 是一个组合 store creator 的高阶函数，返回一个新的强化过的 store creator。这与 middleware 相似，它也允许你通过复合函数改变 store 接口。

* 返回值

1. (Store): 保存了应用所有 state 的对象。改变 state 的惟一方法是 dispatch action。你也可以 subscribe 监听 state 的变化，然后更新 UI。

```js
import { createStore } from 'redux'

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([action.text])
    default:
      return state
  }
}

let store = createStore(todos, ['Use Redux'])

store.dispatch({
  type: 'ADD_TODO',
  text: 'Read the docs'
})

console.log(store.getState())
// [ 'Use Redux', 'Read the docs' ]
```


(2). combineReducers(reducers)

- combineReducers 辅助函数的作用是，把一个由多个不同 reducer 函数作为 value 的 object，合并成一个最终的 reducer 函数，然后就可以对这个 reducer 调用 createStore 方法。
- 合并后的 reducer 可以调用各个子 reducer，并把它们返回的结果合并成一个 state 对象。 由 combineReducers() 返回的 state 对象，会将传入的每个 reducer 返回的 state 按其传递给 combineReducers() 时对应的 key 进行命名。

* 参数 

1. reducers (Object): 一个对象，它的值（value）对应不同的 reducer 函数，这些 reducer 函数后面会被合并成一个。

```js
import { createStore, combineReducers } from 'redux';
import reduxClass from './reducer/class';
import reduxUser from './reducer/user';

const reducer = combineReducers({
  reduxClass,
  reduxUser,
});
const store = createStore(reducer);

export default store;

```


(3). applyMiddleware(...middlewares)

- 执行某个函数之前，可以得到并修改调用这个函数要传的参数，目的就是对参数的校验，加工，或者记录，使这个函数在接收到自己处理不了的参数时，能够在这些中间的函数，将参数转化成自己可以处理的参数，这个过程实际就是redux中间件的工作流程。

* 参数

1. ...middlewares (arguments): 遵循 Redux middleware API 的函数。每个 middleware 接受 Store 的 dispatch 和 getState 函数作为命名参数，并返回一个函数。该函数会被传入 被称为 next 的下一个 middleware 的 dispatch 方法，并返回一个接收 action 的新函数，这个函数可以直接调用 next(action)，或者在其他需要的时刻调用，甚至根本不去调用它。调用链中最后一个 middleware 会接受真实的 store 的 dispatch 方法作为 next 参数，并借此结束调用链。所以，middleware 的函数签名是 ({ getState, dispatch }) => next => action。

* 返回值

1. (Function) 一个应用了 middleware 后的 store enhancer。这个 store enhancer 的签名是 createStore => createStore，但是最简单的使用方法就是直接作为最后一个 enhancer 参数传递给 createStore() 函数。


```js
import { createStore, applyMiddleware } from 'redux'
import todos from './reducers'

function logger({ getState }) {
  return (next) => (action) => {
    console.log('will dispatch', action)

    // 调用 middleware 链中下一个 middleware 的 dispatch。
    let returnValue = next(action)

    console.log('state after dispatch', getState())

    // 一般会是 action 本身，除非
    // 后面的 middleware 修改了它。
    return returnValue
  }
}

let store = createStore(
  todos,
  [ 'Use Redux' ],
  applyMiddleware(logger)
)

store.dispatch({
  type: 'ADD_TODO',
  text: 'Understand the middleware'
})
// (将打印如下信息:)
// will dispatch: { type: 'ADD_TODO', text: 'Understand the middleware' }
// state after dispatch: [ 'Use Redux', 'Understand the middleware' ]
```

(4). bindActionCreators(actionCreators, dispatch)

- bindActionCreators 的场景是当你需要把 action creator 往下传到一个组件上，却不想让这个组件觉察到 Redux 的存在，而且不希望把 dispatch 或 Redux store 传给它。


* 参数

1. actionCreators (Function or Object): 一个 action creator，或者一个 value 是 action creator 的对象。
2. dispatch (Function): 一个由 Store 实例提供的 dispatch 函数。

* 返回值

- (Function or Object): 一个与原对象类似的对象，只不过这个对象的 value 都是会直接 dispatch 原 action creator 返回的结果的函数。如果传入一个单独的函数作为 actionCreators，那么返回的结果也是一个单独的函数。


5. compose(...functions)

- 从右到左来组合多个函数。

```js
import { createStore, combineReducers, applyMiddleware, compose } from 'redux'
import thunk from 'redux-thunk'
import DevTools from './containers/DevTools'
import reducer from '../reducers/index'

const store = createStore(
  reducer,
  compose(
    applyMiddleware(thunk),
    DevTools.instrument()
  )
)
```

### Redux 的 Store 对象方法

1. getState()

- 返回应用当前的 state 树。

2. dispatch(action)

- 分发 action。这是触发 state 变化的惟一途径。

3. subscribe(listener)

- 添加一个变化监听器。每当 dispatch action 的时候就会执行，state 树中的一部分可能已经变化。你可以在回调函数里调用 getState() 来拿到当前 state。

4. replaceReducer(nextReducer)

- 替换 store 当前用来计算 state 的 reducer。

