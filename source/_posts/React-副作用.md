---
title: 副作用
date: 2020-6-20
tags: React
author: 映雪
---

忆君心似西江水，日夜东流无歇时。

<!--more-->

### 纯函数（Pure function）

1. 给函数相同的参数，永远会返回相同的值，并且没有副作用
2. 函数返回结果只依赖它的参数
3. 函数执行过程中不会对外产生可观察的变化

-  这个概念拿到 React 中，就是给一个 Pure component 相同的 props，永远渲染出相同的视图，并且没有其他的副作用

```js
function add(x,y){
let x = x * 10
return x+y
}
```

### 纯函数组件优势

1. 容易监测数据变化
2. 容易测试
3. 提高渲染性能等

### 副作用（Side Effect）

- 副作用是函数在执行过程中对外部造成的影响称之为副作用
- 修改了全局变量、修改了传入的参数等，所以 ajax 操作，修改 dom 都是算作副作用

```js
function add(x,y){
let x = x * 10
let c = Math.random()
return x+y+c
}
```

### React 副作用分离

- 在React hook中，实现了一套让我们在api使用层面可以去不关注副作用，将副作用分离出来，交给内部处理，本质上useState是一个有副作用的api，React内部抽离了，在使用层面上看起来像一个纯函数
- **useEffec这个hook函数就是处理副作用的**

```jsx
const [count, setCount] = useState(0)
useEffect(() => {
}, [count]) 
```

### Effect 收集



- 如果一个 **fiber node** 被标记了 **effect**，那么 **react** 就会在这个 **fiber node** 完成协调以后，将这个 **fiber node** 收集起来。当整颗 **fiber tree** 完成协调以后，所有被标记 **effect** 的 **fiber node** 都被收集到一起来。
- 收集的 **fiber node** 采用**单链表结构存储**，**firstEffect** 指向第一个标记 **effect** 的 **fiber node**，**lastEffect** 标记最后一个 **fiber node**，节点之间通过 **nextEffect** 指针连接。
- 由于 **fiber tree** 协调时采用的顺序是**深度优先**，协调完成的顺序是**子节点、子节点兄弟节点、父节点**，所以收集带 **effect** 标记的 **fiber node**时，顺序也是**子节点、子节点兄弟节点、父节点**。

![截屏2023-06-27 上午11.23.57.png](/images/2023/06/27/eKAmXgOWva4CYfc.png)


### effect 的处理

- fiber tree 协调完成， 带 effect 的 fiber node 收集完毕，接下来要做的就是要处理带 effect 的 fiber node。也就是commit阶段。

![截屏2023-06-27 上午11.17.53.png](/images/2023/06/27/xO1GdinQwkmJD4y.png)

- **fiber tree 协调结束以后，会进入 commit 阶段。在 commit 阶段， react 的主要工作就是处理协调过程中产生的所有的 effect。**

```jsx
commitRoot(root);
```

- 在rootFiber.firstEffect上保存了一条需要执行副作用的Fiber节点的单向链表effectList，这些Fiber节点的updateQueue中保存了变化的props。
- 这些副作用对应的DOM操作在commit阶段执行。
- 除此之外，一些生命周期钩子（比如componentDidXXX）、hook（比如useEffect）需要在commit阶段执行。

- effect 的处理分为三个阶段，这三个阶段按照从前到后的顺序为：

1. before mutation 阶段 (dom 操作之前)
2. mutation 阶段 (dom 操作)
3. layout 阶段 (dom 操作之后)

- 不同的阶段，处理的 effect 种类也不相同。在每个阶段，react 都会从 effect 链表的头部  **firstEffect** 开始，按序遍历 fiber node, 直到 lastEffect。
