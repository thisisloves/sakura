---
title: useState
date: 2020-6-7
tags: React
author: 映雪
---

等闲变却故人心,却道故人心易变。

<!--more-->

### useState

- useState 通过在函数组件里调用它来满足给组件添加一些内部state(状态)，调用useState会返回一个数组：当前状态和修改(更新)状态的函数，调用修改状态的函数来修改状态并触发视图的更新。

### useState 语法

```jsx
const [state, setState] = useState(initialValue);
```
1. state: 用来存储状态的值；
2. setState：修改状态的函数；
3. initialValue：函数式组件第一次渲染时state的初始值。

### useState 更新

1. 直接更新

```
setState(newState)
```

2. 函数式更新

```jsx
setState((preState)=>{return nextState})
```

- preState总能拿到上一次更新成功之后的最新状态

- 区别

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  function handleClick() {
    setTimeout(() => {
      setCount(count + 1)
    }, 3000);
  }
  function handleClickFn() {
    setTimeout(() => {
      setCount((prevCount) => {
        return prevCount + 1
      })
    }, 3000);
  }
  return (
    <>
      Count: {count}
      <button onClick={handleClick}>+</button>
      <button onClick={handleClickFn}>+</button>
    </>
  );
}
```

- ​ 当设置为异步更新，点击按钮延迟到3s之后去调用setCount函数，当快速点击按钮时，也就是说在3s多次去触发更新，但是只有一次生效，因为 count 的值是没有变化的。
​- 当使用函数式更新 state 的时候，这种问题就没有了，因为它可以获取之前的 state 值，也就是代码中的 prevCount 每次都是最新的值。
- 因为使用函数更新的时候，**setCount函数将会被放在一个任务队列中**，每一个setCount函数都可以拿到上一次更新成功后状态值。


### useState自带性能优化

- 每一次修改状态值的时候，会拿最新要修改的值和之前的状态值做比较(基于Object.is做比较)；
- 如果发现两次的值是一样的，则不会修改状态，也不会让视图更新。

### useState 惰性化处理

```jsx
import { useState } from "react";
const Demo = (props) => {
    let { x, y } = props; // 假设父组件传了x 和 y两个类型为number的数据
    let total = 0;
    for (let i = x; i <= y; i++) {total += i;}
    const [num, setNum] = useState(total);
    const handle = () => {setNum(1);};
    return (<><button onClick={handle}>改变</button></>);
};
export default Demo;

```

- 页面每次渲染都会重新执行for循环，就会造成资源浪费

```jsx
let [num, setNum] = useState(() => {
    let { x, y } = props; // 假设父组件传了x 和 y两个类型为number的数据
    let total = 0;
    for (let i = x; i <= y; i++) {total += i;}
    return total;
});
```