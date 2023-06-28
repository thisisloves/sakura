---
title: React生命周期-旧
date: 2019-6-12
tags: React
author: 映雪
---

有思念你的人在的地方，就是你的归处。

<!--more-->

### 生命周期

- React 从开始创建、初始化数据、编译模板、渲染、更新 、准备销毁、销毁 等一系列过程就是生命周期。

![1106982-20170811224737742-1564011484.jpg](/images/2019/11/24/RHfqECQz6PZn9tU.jpg)

### getDefaultProps()

- 执行过一次后，被创建的类会有缓存，映射的值会存在 this.props，前提是这个 prop 不是父组件指定的。
- 这个方法在对象被创建之前执行，因此不能在方法内调用 this.props ，另外，注意任何 getDefaultProps()返回的对象在实例中共享，不是复制。

### getInitialState()

- 控件加载之前执行，返回值会被用于 state 的初始化值。

### componentWillMount()

- 执行一次，在初始化 render 之前执行，如果在这个方法内调用 setState，render()知道 state 发生变化，并且只执行一次。

### render()

- 调用 render()方法时，首先检查 this.props 和 this.state 返回一个子元素，子元素可以是 DOM 组件或者其他自定义复合控件的虚拟实现。
- 如果不想渲染可以返回 null 或者 false，这种场景下，react 渲染一个<noscript>标签，当返回 null 或者 false 时，ReactDOM.findDOMNode(this)返回 null。
- render()方法是很纯净的，这就意味着不要在这个方法里初始化组件的 state，每次执行时返回相同的值，不会读写 DOM 或者与服务器交互，如果必须如服务器交互，在 componentDidMount()方法中实现或者其他生命周期的方法中实现，保持 render()方法纯净使得服务器更准确，组件更简单。

### componentDidMount()

- 在初始化 render 之后只执行一次，在这个方法内，可以访问任何组件，componentDidMount()方法中的子组件在父组件之前执行。
- 从这个函数开始，就可以和 js 其他框架交互了，例如设置计时 setTimeout 或者 setInterval，或者发起网络请求。

### shouldComponentUpdate（）

```js
boolean shouldComponentUpdate(
  object nextProps, object nextState
}
```

- 这个方法在初始化 render 时不会执行，当 props 或者 state 发生变化时执行，并且是在 render 之前，当新的 props 或者 state 不需要更新组件时，返回 false。

```js
shouldComponentUpdate: function(nextProps, nextState) {
  return nextProps.id !== this.props.id;
}
```

- 当 shouldComponentUpdate 方法返回 false 时，就不会执行 render()方法，componentWillUpdate 和 componentDidUpdate 方法也不会被调用。

- 默认情况下，shouldComponentUpdate 方法返回 true 防止 state 快速变化时的问题，但是如果·state 不变，props 只读，可以直接覆盖 。shouldComponentUpdate 用于比较 props 和 state 的变化，决定 UI 是否更新，当组件比较多时，使用这个方法能有效提高应用性能。

### componentWillUpdate

```js
void componentWillUpdate(
  object nextProps, object nextState
)
```

- 当 props 和 state 发生变化时执行，并且在 render 方法之前执行，当然初始化 render 时不执行该方法，需要特别注意的是，在这个函数里面，你就不能使用 this.setState 来修改状态。这个函数调用之后，就会把 nextProps 和 nextState 分别设置到 this.props 和 this.state 中。紧接着这个函数，就会调用 render()来更新界面了。

### componentDidUpdate

```js
void componentDidUpdate(
  object prevProps, object prevState
)
```

- 组件更新结束之后执行，在初始化 render 时不执行。

### componentWillReceiveProps()

```js
void componentWillReceiveProps(
  object nextProps
)
```

1. 组件初次渲染时不会执行 componentWillReceiveProps。
2. 当 props 发生变化时执行 componentWillReceiveProps。
3. 在这个函数里面，旧的属性仍可以通过 this.props 来获取。
4. 此函数可以作为 react 在 prop 传入之后， render() 渲染之前更新 state 的机会。即可以根据属性的变化，通过调用 this.setState()来更新你的组件状态，在该函数中调用 this.setState() 将不会引起第二次渲染。

```js
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
    return <div>父传递过来的参数:{this.state.a}</div>;
  }
}

export default List;
```

### componentWillUnmount()

- 当组件要被从界面上移除的时候，就会调用 componentWillUnmount()，在这个函数中，可以做一些组件相关的清理工作，例如取消计时器、网络请求等。
