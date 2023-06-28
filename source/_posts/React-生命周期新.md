---
title: React生命周期-新
date: 2019-6-13
tags: React
author: 映雪
---

桃李春风一杯酒，江湖夜雨十年灯。

<!--more-->

### 生命周期

- React 从开始创建、初始化数据、编译模板、渲染、更新 、准备销毁、销毁 等一系列过程就是生命周期。

![截屏2022-04-11 下午4.02.11.png](/images/2022/04/11/mW6ULXlbVkhHA8I.png)

### constructor()

- 在构造函数里初始化 state 对象或者给自定义方法绑定 this。

### static getDerivedStateFromProps()

- getDerivedStateFromProps 会在调用 reder 方法之前调用，并且在初始挂载及后续更新时都会被调用，它应该返回一个对象来更新 state，如果返回 null 则不更新任何内容。

```js
  static getDerivedStateFromProps(props, state) {
    console.log(props, state);
    if (!state.b) {
      return { a: props.num };
    }
    return null;
  }
```

### render()

- 渲染 DOM。

### componentDidMount()

- 在初始化 render 之后只执行一次，在这个方法内，可以访问任何组件，componentDidMount()方法中的子组件在父组件之前执行。
- 从这个函数开始，就可以和 js 其他框架交互了，例如设置计时 setTimeout 或者 setInterval，或者发起网络请求。

### shouldComponentUpdate()

- 根据 shouldComponentUpdate()的返回值，判断 React 组件的输入是否受当前 state 或者 props 更改的影响。
- 默认行为是 state 每次发生改变组件都会重新渲染。
- **这个生命周期的钩子函数是提升 react 性能的关键**
- 当状态 state 或者属性 props 发生改变，当前的组件视图是不是需要更新，默认的返回值是 true，要不不写，要么必须写返回值，返回值必须为 boolean 类型。
- 如果返回值为 false，生命周期的钩子函数调用就到此为止。

### getSnapshotBeforeUpdate()

- getSnapshotBeforeUpdate()在最近一次渲染输入(提交到 DOM 节点)之前调用。它使得组件能够在发生改变之前从 DOM 中捕获一些信息(例如滚动位置)。获取 render 之前的 DOM 状态。

### componentDidUpdate()

- componentDidUpdate()会在更新后立即调用。首次渲染不会执行此方法。可以执行 DOM 操作。

### componentWillUnmount()

- componentWillUnmount()会在组件卸载及销毁之前直接调用。在此方法中执行必要的清理操作，例如，清除 timer，取消网络请求或者清除在 componentDidMount()中创建的订阅等。

### static getDerivedStateFromErro

- 它会在其子组件树中的任何位置捕获 Javascript 错误，并记录这些错误，展示降级 UI 而不是崩溃的组件树。

### 弃用的生命周期

- componentWillMount
- componentWillReceiveProps
- componentWillUpdate
