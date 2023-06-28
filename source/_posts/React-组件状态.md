---
title: setState 新旧版本同异步对比
date: 2019-9-10
tags: React
author: 映雪
---

问君能有几多愁，恰似一江春水向东流。

<!--more-->

### state 和 props 之间的区别

- props 是传递给组件的（类似于函数的形参），而 state 是在组件内被组件自己管理的（类似于在一个函数内声明的变量）。

### setState

```js
setState(updater, [callback]);
```

- setState() 将对组件 state 的更改排入队列，并通知 React 需要使用更新后的 state 重新渲染此组件及其子组件。这是用于更新用户界面以响应事件处理器和处理服务器数据的主要方式

- 将 setState() 视为请求而不是立即更新组件的命令。为了更好的感知性能，React 会延迟调用它，然后通过一次传递更新多个组件。React 并不会保证 state 的变更会立即生效。

- setState() 并不总是立即更新组件。它会批量推迟更新。这使得在调用 setState() 后立即读取 this.state 成为了隐患。为了消除隐患，请使用 componentDidUpdate 或者 setState 的回调函数（setState(updater, callback)）。

### react 不同步地更新 this.state 原因

1. 同步更新会破坏掉 props 和 state 之间的一致性，造成一些难以 debug 的问题。
2. 可以显著提升性能，如果每次调用 setState 都进行一次更新，那么意味着 render 函数会被频繁调用，界面重新渲染，这样效率是很低的，所以最好的办法是获取到多个更新，然后进行批量的更新

### setState 使用

- **setState 代码本身是同步的，但是调用 setState 其实是异步的** ，不要指望在调用 setState 之后，this.state 会立即映射为新的值。

```js
import React from 'react';
import { Button } from 'antd';

class StateComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
    };
  }

  increment = () => {
    console.log('increment setState前的count', this.state.count);
    this.setState({
      count: this.state.count + 1,
    });
    console.log('increment setState 后的count', this.state.count);
  };
  triple = () => {
    console.log('triple setState前的count', this.state.count);
    this.setState({
      count: this.state.count + 1,
    });
    console.log('triple setState后的count', this.state.count);

    this.setState({
      count: this.state.count + 1,
    });
    console.log('triple setState后的count', this.state.count);

    this.setState({
      count: this.state.count + 1,
    });
    console.log('triple setState后的count', this.state.count);
  };
  reduce = () => {
    setTimeout(() => {
      console.log('reduce setState前的count', this.state.count);
      this.setState({
        count: this.state.count - 1,
      });
      console.log('reduce setState后的count', this.state.count);
    }, 10);
  };

  componentDidMount() {
    document.getElementsByClassName('button')[0].addEventListener('click', (e) => {
      this.handleClick(e);
    });
  }
  handleClick() {
    console.log('原生点击事件 setState 前的count', this.state.count);
    this.setState({
      count: this.state.count + 1,
    });
    console.log('原生点击事件 setState后的count', this.state.count);
  }
  componentWillUnmount() {
    document.getElementsByClassName('button')[0].removeEventListener('click');
  }
  render() {
    return (
      <div>
        <Button onClick={this.increment}>点我增加</Button>
        <Button onClick={this.triple}>点我增加三倍</Button>
        <Button onClick={this.reduce}>seTimeOut减一</Button>
        <Button className="button">原生点击事件加一</Button>
      </div>
    );
  }
}

export default StateComponent;
```

- **react 16.8.0 中执行的结果**

![截屏2022-05-09 下午6.25.56.png](/images/2022/05/09/WS4kYfLvzmgBJnh.png)

- 在这个版本及之前版本中，setState 在合成事件和钩子函数中是异步的，而在原生事件和 setTimeout 中都是同步的。

- **react 18.0.0 中执行的结果**

![截屏2022-05-09 下午6.21.26.png](/images/2022/05/09/l3Ozo4RExpUTWfM.png)

- 在这个版本中，setState 在合成事件和钩子函数以及原生事件和 setTimeout 中都是异步的。

### setState 原理

- 主要是通过队列机制实现 state 更新，当执行 setState 的时候，会将需要更新的 state 合并后放入状态队列中，而不会立即更新 this.state。队列机制主要实现了批量更新 state。
- 如果**不使用 setState 更新，而是直接采用 this.state 来修改，state 不会被放入状态队列中**，下次调用 setState 时对状态队列进行合并时，会忽略之前修改的 state，就不会达到预期效果。

![截屏2022-05-09 下午6.37.58.png](/images/2022/05/09/KR94FIGvOYPjSqM.png)

- 在 16.8.0 版本中，在 React 的生命周期和合成事件执行前后都有相应的钩子，分别是 pre 钩子和 post 钩子，pre 钩子会调用 batchedUpdate 方法将 isBatchingUpdates 变量置为 true，开启批量更新，而 post 钩子会将 isBatchingUpdates 置为 false。而原生事件和异步操作中，不会执行 pre 钩子，或者生命周期的中的异步操作之前执行了 pre 钩子，但是 pos 钩子也在异步操作之前执行完了，isBatchingUpdates 必定为 false，也就不会进行批量更新。

- 最新版 18.0.0 中 则完全都是**进入状态队列，所有 state 更新表现为异步更新**。
