---
title: static getDerivedStateFromProps
date: 2019-6-15
tags: React
author: 映雪
---

玉楼金阙慵归去，且插梅花醉洛阳。

<!--more-->

### getDerivedStateFromProps

- 将传入的 props 映射到 state 上面。
- getDerivedStateFromProps 会在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 state，如果返回 null 则不更新任何内容。

```js
  static getDerivedStateFromProps(props, state) {
    console.log(props, state);
  }
```

### getDerivedStateFromProps 执行时机

1. 挂载

```js
construtor > static getDerivedStateFromProps > render > componentDidMount
```

2. 更新

```js
static getDerivedStateFromProps  > shoudComponentUpdate > render > componentDidUpdate
```

### getDerivedStateFromProps 执行条件

1. 父组件 props 改变。（只要父级重新渲染时，生命周期函数就会重新调用，不管 props 有没有“变化”。）
2. 所在组件 state 改变。

### getDerivedStateFromProps 使用

- getDerivedStateFromProps 是一个静态函数，也就是这个函数不能通过 this 访问到 class 的属性，也并不推荐直接访问属性。而是应该通过参数提供的 nextProps 以及 prevState 来进行判断，根据新传入的 props 来映射到 state。
- 如果 props 传入的内容不需要影响到你的 state，那么就需要返回一个 null，这个返回值是必须的，所以尽量将其写到函数的末尾。

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
  static getDerivedStateFromProps(props, state) {
    console.log(props, state);
    return { a: props.num };
  }
  render() {
    return <div>父传递过来的参数:{this.state.a}</div>;
  }
}

export default List;
```

- 问题： 当子组件 state 修改时会触发 getDerivedStateFromProps 导致无法修改

```js
import { Button } from 'antd';
import React from 'react';

class List extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      a: 1,
      b: false,
    };
  }

  static getDerivedStateFromProps(props, state) {
    console.log(props, state);
    if (!state.b) {
      return { a: props.num };
    }
    return null;
  }
  render() {
    return (
      <div>
        父传递过来的参数:{this.state.a}
        <Button onClick={() => this.setState({ a: 222, b: true })}>修改子组件state</Button>
      </div>
    );
  }
}

export default List;
```

- 加一个条件
