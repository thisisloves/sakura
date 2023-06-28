---
title: useEffect
date: 2020-6-8
tags: React
author: 映雪
---

人生若只如初见，何事秋风悲画扇。

<!--more-->

### React 渲染

-  当我们更新 state 的时候，React会重新渲染组件。**每一次渲染都能拿到独立的 state**，这个状态值是函数中的一个常量。

```jsx
function Counter() {
  const [count, setCount] = useState(0);
 
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });
 
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}

```

- effect是如何读取到最新的 count 状态值的呢？

- **并不是 count的值在“不变”的 effect 中发生了改变，而是 effect 函数本身在每一次渲染中都不相同。**
- **React 会记住你提供的 effect 函数，并且会在每次更改作用于DOM并让浏览器绘制屏幕后去调用它。所以虽然我们说的是一个 effect（这里指更新document的title），但其实每次渲染都是一个不同的函数 ，并且每个 effect 函数看到的 props 和 state 都来自于它属于的那次特定渲染。**

### useEffect

- useEffect 可以让我们在函数组件中执行 **副作用**操作
- useEffect逻辑处理函数会在组件挂载时执行一次，是相当于componentDidMount 和 componentDidUpdate 的结合。
- useEffect第一个参数可以返回一个回调函数，这个回调函数将会在组件被摧毁之前和再一次触发更新时，将之前的副作用清除掉。这就相当于componentWillUnmount。

1. componentDidMount 组件挂载
2. componentDidUpdate 组件更新
3. componentWillUnmount 组件将要摧毁

### 副作用

- 副作用 ( side effect ): 数据获取，数据订阅，以及手动更改 React 组件中的 DOM 都属于副作用。
- 因为我们渲染出的页面都是静态的，任何在其之后的操作都会对他产生影响，所以称之为副作用。


1. 无需清除的副作用：

- 有时候，我们只想在 React 更新 DOM 之后运行一些额外的代码。比如发送网络请求，手动变更 DOM，记录日志，这些都是常见的无需清除的操作。因为我们在执行完这些操作之后，就可以忽略他们了。

2. 需要清除的副作用：

- 有一些副作用是需要清除的。例如订阅外部数据源，添加DOM事件。这种情况下，清除工作是非常重要的，可以防止引起内存泄露。


### useEffect 用法

```jsx
useEffect(() => {
/** 执行逻辑 */
},[])
```

1. 第二个参数存放变量，当数组存放变量发生改变时，第一个参数，逻辑处理函数将会被执行
2. 第二个参数可以不传，不会报错，但浏览器会无线循环执行逻辑处理函数。


### useEffect 使用情景

1. 不传递

```jsx
useEffect(()=>{
console.log(props.number)
setNumber(props.number)
}) 
```

- useEffect不传递第二个参数会导致每次渲染都会运行useEffect。然后，当它运行时，它获取数据并更新状态。然后，一旦状态更新，组件将重新呈现，这将再次触发useEffect。

2. 传递空数组

```jsx
useEffect(()=>{
console.log(props)
},[]) 
```

- 传递空数组时，仅在挂载和卸载的时候执行

3. 数组传递一个值

```jsx
useEffect(()=>{
console.log(count)
},[count]) 

```
- 数组传递一个值时，只有依赖值更新时才会执行

4. 数组传递多个值 

```jsx
const [a, setA] = useState(1);
const [b, setB] = useState(2);
useEffect(() => {
/** 执行逻辑 */
},[a,b])
```
- 数组传递多个值时，只要有一个依赖值更新时就执行

5. return 方法

```jsx
const timer = setInterval(() => {
setCount(count + 1)
}, 1000)

useEffect(() => {
return () => {
clearInterval(timer)
}
}, [])
```

- useEffect第一个参数可以返回一个回调函数，这个回调函数将会在组件**被摧毁之前**和**再一次触发更新**时，将之前的副作用清除掉。这就相当于componentWillUnmount。

### useEffect 执行机制

1. 首先渲染，并不会执行useEffect中的 return
2. 变量修改后，**导致的重新render，会先执行 useEffect 中的 return**，再执行useEffect内除了return部分代码。
3. return 内的回调，可以用来清理遗留垃圾，比如订阅或计时器 ID 等占用资源的东西。

