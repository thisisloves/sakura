---
title: Redux使用
date: 2019-7-9
tags: Redux
author: 映雪
---

顷觉日已晚,归思且为安。

<!--more-->

### 安装依赖

```js
npm install react-redux --save
npm isntall redux --save
npm install redux-thunk
```

### 创建 redux 文件夹

![截屏2022-04-20 下午4.48.02.png](/images/2022/04/20/Tfs64CKrqUbEvpd.png)

### stroe 仓库创建

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

### reducer 编写

- user.js

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

### 组件连接 store

- Provider 是一个组件,用于连接了 Store,它把 store 提供给内部组件,接受 store 作为 props,然后通过 context 往下传，这样 react 中任何组件都可以通过 context 获取 store，只要被这个 Provider 组件包裹了,那么它内部的子组件就有能力接收到 store，内部的组件都有能力获取 store 的数据的。
- Provider 其实是对 Redux 中的 store 的 subscribe,dispatch,getState 的一个封装,集成.它对外暴露 props 属性,内部却已经帮我们实现了的 react-redux 提供 Provider 组件，可以让容器组件拿到 state。

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { Provider } from 'react-redux'; // 从react-redux库中引入Provider
import store from './store'; // 引入store

const container = document.getElementById('root');

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  container,
);
```

### class 组件中调用

```js
import React from 'react';
import styles from './index.less';
import { Button } from 'antd';
import { connect } from 'react-redux';

class Home extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  }

  render() {
    return (
      <div className={styles.home}>
        <p>这是redux里面reduxUser模块的number:{this.props.reduxUser.number}</p>
        <Button type="primary" onClick={() => this.props.dispatch({ type: 'addCount', count: 2 })}>
          修改redux
        </Button>
      </div>
    );
  }
}

export default connect((state) => ({
  reduxUser: state.reduxUser,
}))(Home);
```

### 函数组件调用

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
      <Button type="primary" onClick={() => dispatch({ type: 'addCount', count: 2 })}>
        修改redux
      </Button>
    </div>
  );
};

export default ReduxUser;
```


### 使用mapStateToProps和mapDispatchToProps

```js
import React from 'react';
import styles from './index.less';
import { Button } from 'antd';
import { connect } from 'react-redux';

const mapDispatchToProps = (dispatch) => {
  return {
    fixNumber(num) {
      dispatch({
        type: 'addCount',
        count: num,
      });
    },
  };
};
const mapStateToProps = (state) => {
  return {
    number: state.reduxUser.number,
  };
};

class TableCom extends React.Component {
  static staticNum = 11;
  constructor(props) {
    super(props);
    this.state = {};
  }
  componentDidMount() {
    console.log(this.props);
  }

  render() {
    return (
      <div className={styles.table}>
        <p>这是redux里面reduxUser模块的number:{this.props.number}</p>
        <Button type="primary" onClick={() => this.props.fixNumber(2)}>
          修改redux
        </Button>
      </div>
    );
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(TableCom);
```