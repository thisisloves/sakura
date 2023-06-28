---
title: Redux-saga使用
date: 2019-11-15
tags: Redux
author: 映雪
---

山长水远嫦娥怨，鸿雁相烦，鸿雁相烦，眉间心上玉簟寒。

<!--more-->

### Redux-saga

- redux-saga 是一个 redux 的中间件，而中间件的作用是为 redux 提供额外的功能。
- 由于在 reducers 中的所有操作都是同步的并且是纯粹的，即 reducer 都是纯函数，纯函数是指一个函数的返回结果只依赖于它的参数，并且在执行过程中不会对外部产生副作用，即给它传什么，就吐出什么。
- 但是在实际的应用开发中，我们希望做一些异步的（如 Ajax 请求）且不纯粹的操作（如改变外部的状态），这些在函数式编程范式中被称为“副作用”。
- redux-saga 是 generator 函数进行构建的。

### Redux-saga 使用

1. 安装

```js
npm i -S react-redux redux-saga
```

2. 引入 saga 中间件，创建 saga 文件

```js
import { createStore, combineReducers, applyMiddleware, compose } from 'redux';
import reduxClass from './reducer/class';
import reduxUser from './reducer/user';
import createSagaMiddleware from 'redux-saga';
import saga from './saga.js';

const sagaMiddleware = createSagaMiddleware();

const reducer = combineReducers({
  reduxClass,
  reduxUser,
});
const store = createStore(reducer, applyMiddleware(sagaMiddleware));

sagaMiddleware.run(saga);
export default store;
```

3. reducer 编写

```js
const init = {
  number: 0,
};
//eslint-disable-next-line
export default (state = init, action) => {
  switch (action.type) {
    case 'addCount':
      return { ...state, number: state.number + action.count };
    case 'reduceCount':
      return { ...state, number: state.number - action.count };
    default:
      return state;
  }
};
```

4. saga.js 编写

```js
import { call, delay, put, all, takeEvery } from 'redux-saga/effects';

function getCount(ms) {
  const promise = () =>
    new Promise(function (resolve, reject) {
      setTimeout(() => {
        resolve(true);
      }, ms);
    });
  return promise();
}

function* fixNumber(payload) {
  const randomNumber = yield call(getCount, 1000);
  // yield delay(1000);
  if (randomNumber) {
    yield put({ type: 'addCount', count: payload.count });
  }
}

// 监听派发
function* watchAdd() {
  yield takeEvery('asyAddCount', fixNumber);
}

export default function* rootSage() {
  yield all([watchAdd()]);
}
```

5. 组件中使用

```js
import React, { useRef } from 'react';
import { Button } from 'antd';
import { useSelector, useDispatch } from 'react-redux';

const ReduxUser = (props) => {
  const number = useSelector((state) => state.reduxUser.number);
  const dispatch = useDispatch();

  return (
    <div>
      <p>这是redux里面reduxUser模块的number:{number}</p>
      <Button
        type="primary"
        onClick={() =>
          setTimeout(() => {
            dispatch({ type: 'addCount', count: 2 });
          }, 1000)
        }
      >
        修改redux
      </Button>

      <Button type="primary" onClick={() => dispatch({ type: 'asyAddCount', count: 3 })}>
        通过redux-saga修改
      </Button>
    </div>
  );
};

export default ReduxUser;
```

- 通过 redux-saga 修改相当于代理了一个 action，返回一个处理后的 action，监听 asyAddCount 类型的 dispatch，处理后再派发 addCount 类型

### Redux-saga 流程

1. 组件中调用了 dispatch({ type: 'asyAddCount', count: 3 }),返回的 action 对象会在 connect 方法中被派发
2. saga 中 takeEvery('asyAddCount', fixNumber),监听到动作的类型后，触发 worker saga =>fixNumber
3. worker saga 中先 请求数据
4. yield put({type:addCount,count:payload.count}); 最后派发的还是 addCount 的类型
5. 接着被 reducer 处理，更新 state
6. 页面刷新
