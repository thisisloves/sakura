---
title: React-redux
date: 2019-9-20
tags: Redux
author: 映雪
---

愿你有前进一寸的勇气，亦有退后一尺的从容。

<!--more-->

### react-redux

- 搭配 Redux 一起在 react 的项目中使用。

### react-redux 安装

```
npm install --save react-redux
```

### react-redux 的 API

1. `<Provider store>`

- `<Provider/>`为后代组件提供 store，使组件层级中的 connect()方法都能够获得 Redux store. 正常情况下,你的根组件应该嵌套在<Provider>中才能使用 connect()的方法。

- 原理：跨级传递数据给下级

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

2. connect([mapStateToProps],[mapDispatchToProps],[mergeProps],[options])

- 连接 react 组件与 Redux store. 返回一个新的已与 Redux store 连接的组件类。
- 原理：高阶函数，对组件包装，返回一个新的组件类。

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

### react-redux 中 hooks 的 API

1. useSelector 获取 store state

2. useDispatch 获取 dispatch

```js
import React, { useRef } from 'react';
import { Button } from 'antd';
import { useSelector, useDispatch } from 'react-redux';

const ReduxUser = (props) => {
  const number = useSelector((state) => state.reduxUser.number);
  const dispatch = useDispatch()
  return (
    <div>
      <p>这是redux里面reduxUser模块的number:{number}</p>
      <Button type="primary" onClick={()=>
      setTimeout(()=>{
        dispatch({type:'addCount',count:2})
      },1000)
        }>修改redux</Button>
    </div>
  );
};

export default ReduxUser;

```