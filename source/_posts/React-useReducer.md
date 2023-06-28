---
title: useReducer
date: 2020-6-14
tags: React
author: 映雪
---

骊山语罢清宵半，泪雨霖铃终不怨。

<!--more-->

### reducer

- reducer是redux中一个概念，以下就是redux更新数据的流程。

![截屏2023-06-05 上午10.42.40.png](/images/2023/06/05/gaoLmR89WeQ6lVK.png)

- redux有复杂的概念以及较高的维护成本

1. 难以维护的Action
2. 复杂度无法预知的Store
3. 复杂到令人绝望的Reducer

### useReducer

- useReducer只是一个小范围内的状态管理工具，它是用来在某些特殊场景下，来取代useState的，它把redux的复杂度控制在了可以接受的范围之内。
- **useState 实际上执行的也是一个 useReducer**， **useReducer 是更原生的**，可以在任何使用 useState 的地方都替换成使用 useReducer。

### useReducer 语法

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

- 参数

1. 第一个是 reducer 函数，这个函数返回一个新的state。
2. 第二个是 state 初始值。
3. 第三个是 初始化函数。

### useReducer 简单使用

```jsx
const initialState = {count: 0};
 
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}
 
function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}

```

- 惰性初始化

```jsx
import { Button } from 'antd';
import React, { useReducer } from 'react';

const init=(value) => {return {count:value}}

function reducer(state, action) {
    switch (action.type) {
        case "add":
            return { count: state.count + 1 }
        case "reduce":
            return { count: state.count - 1 }
        case 'reset':
            return init(action.payload)
        default:
            throw new Error()
    }
}

const App = ({initialCount = 0}) => {
    const [state, dispath] = useReducer(reducer, initialCount,init);
    return <>
        <p>count:{state.count}</p>
        <Button onClick={(() => dispath({ type: 'add' }))}>添加+1</Button>
        <Button onClick={(() => dispath({ type: 'reduce' }))}>减少-1</Button>
        <Button onClick={(() => dispath({ type: 'reset',payload:initialCount }))}>reset</Button>
    </>
}

export default App;
```


### useReducer 使用场景

- state是一个数组或者对象。
- state变化很复杂，经常一个操作需要修改很多state。
- 应用程序比较大，希望UI和业务能够分开维护。


### useReducer 例子

```jsx
function LoginPage() {
  const [name, setName] = useState(''); // 用户名
  const [pwd, setPwd] = useState(''); // 密码
  const [isLoading, setIsLoading] = useState(false); // 是否展示loading，发送请求中
  const [error, setError] = useState(''); // 错误信息
  const [isLoggedIn, setIsLoggedIn] = useState(false); // 是否登录

  const login = (event) => {
    event.preventDefault();
    setError('');
    setIsLoading(true);
    login({ name, pwd })
      .then(() => {
      setIsLoggedIn(true);
      setIsLoading(false);
    })
      .catch((error) => {
      // 登录失败: 显示错误信息、清空输入框用户名、密码、清除loading标识
      setError(error.message);
      setName('');
      setPwd('');
      setIsLoading(false);
    });
  }
}

```

- 使用useReducer

```jsx
const initState = {
    name: '',
    pwd: '',
    isLoading: false,
    error: '',
    isLoggedIn: false,
}
function loginReducer(state, action) {
    switch(action.type) {
        case 'login':
            return {
                ...state,
                isLoading: true,
                error: '',
            }
        case 'success':
            return {
                ...state,
                isLoggedIn: true,
                isLoading: false,
            }
        case 'error':
            return {
                ...state,
                error: action.payload.error,
                name: '',
                pwd: '',
                isLoading: false,
            }
        default: 
            return state;
    }
}
function LoginPage() {
    const [state, dispatch] = useReducer(loginReducer, initState);
    const { name, pwd, isLoading, error, isLoggedIn } = state;
    const login = (event) => {
        event.preventDefault();
        dispatch({ type: 'login' });
        login({ name, pwd })
            .then(() => {
                dispatch({ type: 'success' });
            })
            .catch((error) => {
                dispatch({
                    type: 'error'
                    payload: { error: error.message }
                });
            });
    }
    return ( 
        //  返回页面JSX Element
    )
}

```
