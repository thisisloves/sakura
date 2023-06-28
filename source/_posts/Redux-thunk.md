---
title: Redux-thunk
date: 2019-10-9
tags: Redux
author: 映雪
---

既不回头，何必不忘。既然无缘，何须誓言。今日种种，似水无痕。明夕何夕，君已陌路。

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

### Redux-thunk 原理

- 默认情况下的 dispatch(action)，action 需要是一个 JavaScript 的对象。
- redux-thunk 可以让 dispatch(action 函数), action 可以是一个函数。
- 该函数会被调用, 并且会传给这个函数两个参数: 一个 dispatch 函数和 getState 函数

1. dispatch 函数用于我们之后再次派发 action
2. getState 函数考虑到我们之后的一些操作需要依赖原来的状态，用于让我们可以获取之前的一些状态

### Redux-thunk 使用

```js
import { createStore, combineReducers, applyMiddleware } from 'redux';
import reduxClass from './reducer/class';
import reduxUser from './reducer/user';
import thunk from 'redux-thunk';

const reducer = combineReducers({
  reduxClass,
  reduxUser,
});

const store = createStore(reducer, applyMiddleware(thunk));

export default store;
```

- 定义返回函数的 action。

```jsx
import React, { useRef } from 'react';
import { Button } from 'antd';
import { useSelector, useDispatch } from 'react-redux';

const ReduxUser = (props) => {
  const number = useSelector((state) => state.reduxUser.number);
  const dispatch = useDispatch();

  // action
  const fixNumber = (dispatch, getSate) => {
    setTimeout(() => {
      dispatch({ type: 'addCount', count: 2 });
    }, 1000);
  };
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

      <Button type="primary" onClick={() => dispatch(fixNumber)}>
        使用redux-thunk修改redux
      </Button>
    </div>
  );
};

export default ReduxUser;
```

- 两种修改从功能层面上来说，两者并无差别，都可以满足业务场景需求。后者更加优雅。


### Redux-thunk 源码

```js

function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }
 
    return next(action);
  };
}
 
const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;
 
export default thunk;
```


### redux-thunk的缺点

- thunk仅仅做了执行这个函数，并不在乎函数主体内是什么，也就是说thunk使得redux可以接受函数作为action，但是函数的内部可以多种多样。