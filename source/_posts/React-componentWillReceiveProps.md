---
title: componentWillReceiveProps
date: 2019-6-14
tags: React
author: 映雪
---

或许前路永夜，即便如此我也要前进，因为星光即使微弱，也会为我照亮前路。

<!--more-->


### componentWillReceiveProps 

- componentWillReceiveProps 是组件的一个很重要的生命周期函数。

```js
componentWillReceiveProps(nextProps,nextContext){
        console.log(`this.props-->${JSON.stringify(this.props)}`);   // 旧的props
        console.log(`nextProps-->${JSON.stringify(nextProps)}`);     // 新的props
        console.log(`nextContext-->${JSON.stringify(nextContext)}`);
    }
```

1. 组件初次渲染时不会执行 componentWillReceiveProps；
2. 当 props 发生变化时执行 componentWillReceiveProps；
3. 在这个函数里面，旧的属性仍可以通过 this.props 来获取；
4. 此函数可以作为 react 在 props 传入之后， render() 渲染之前更新 state 的机会。


### componentWillReceiveProps 使用


```js
import { Button } from 'antd';
import React from 'react';

class List extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      a: 1,
    };
  }

    componentDidMount() {
      this.setState({ a: this.props.num });
    }
    componentWillReceiveProps(nextPorps) {
      this.setState({
        a: nextPorps.num,
      });
    }
  render() {
    return (
      <div>
        父传递过来的参数:{this.state.a}
      </div>
    );
  }
}

export default List;

```

- componentWillReceiveProps 会传递最新的props，从而更新state。


### componentWillReceiveProps 缺陷

1. props不变也会触发。当父组件重新渲染，子组件的componentWillReceiveProps即会触发。
2. 有时我们的state是依赖于props，因为我们setState可能是在这个时候执行。但注意setState这个操作是个异步的，而state变化也会触发新的render。
3. componentWillReceiveProps中如果执行副作用，存在内存溢出的风险，比如发起异步action，更新redux状态数据，进而引发组件props更新，循环触发componentWillReceiveProps。