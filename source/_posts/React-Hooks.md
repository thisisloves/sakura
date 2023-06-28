---
title: React-Hooks
date: 2020-6-1
tags: React
author: 映雪
---

我们用零距离的爱，弹奏一曲琴韵笙箫，将生命畅想成天籁之音。

<!--more-->

### Hooks

- Hooks 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。
- 类组件是一种面向对象思想的体现，类组件之间的状态会随着功能增强而变得越来越臃肿，代码维护成本也比较高，而且不利于后期 tree shaking。所以有必要做出一套函数组件代替类组件的方案，这就是Hooks的由来

![截屏2022-05-10 下午5.09.03.png](/images/2022/05/10/ySU3TtHqDJaimwZ.png)

- dispatcher 是包含了 hooks 函数的共享对象，Hooks queue 用顺序下链接在一起的节点。它的初始状态在首次渲染时被创建。它的状态可以即时更新。

### Hooks 限制

1. **不在循环、条件或嵌套函数中使用 Hooks**
2. **只能在函数时组件中调用 Hooks**

- React Hooks 是**基于数组实现**的，如果在循环、条件或嵌套函数中使用 Hooks，可能会造成取值错位等错误发生。


### Fiber

- React16 以前，对 virtural dom 的更新和渲染是同步的，大量的的同步计算任务阻塞了浏览器的 UI 渲染。默认情况下，JS 运算、页面布局和页面绘制都是运行在浏览器的主线程当中，他们之间是互斥的关系。如果 JS 运算持续占用主线程，页面就没法得到及时的更新。当我们调用 setState 更新页面的时候，React 会遍历应用的所有节点，计算出差异，然后再更新 UI。如果页面元素很多，整个过程占用的时机就可能超过 16 毫秒，就容易出现掉帧的现象。

![截屏2022-05-05 下午2.49.29.png](/images/2022/05/05/TxeDFdjNmXorywv.png)

- fiber 把一个任务分成很多小片，当分配给这个小片的时间用尽的时候，就检查任务列表中有没有新的、优先级更高的任务，有就做这个新任务，没有就继续做原来的任务。这种方式被叫做异步渲染(Async Rendering)。

![截屏2022-05-05 下午2.49.38.png](/images/2022/05/05/jGmO7u8PLhc6bRw.png)

- 把一个耗时长的任务分成很多小片，每一个小片的运行时间很短，虽然总时间依然很长，但是在每个小片执行完之后，都给其他任务一个执行的机会，这样唯一的线程就不会被独占，其他任务依然有运行的机会。
- React Fiber 把更新过程碎片化，执行过程如下面的图所示，每执行完一段更新过程，就把控制权交还给 React 负责任务协调的模块，看看有没有其他紧急任务要做，如果没有就继续去更新，如果有紧急任务，那就去做紧急任务。

- Fiber 数据结构

```js
Fiber = {
  tag, // 标记不同的组件类型
  key, // ReactElement里面的key
  elementType, // ReactElement.type，也就是我们调用`createElement`的第一个参数
  type, // 一般是`function`或者`class`
  stateNode, // 在浏览器环境，就是DOM节点

  return, // 指向他在Fiber节点树中的`parent`，用来在处理完这个节点之后向上返回
  child, // 单链表树结构，指向自己的第一个子节点
  sibling, // 指向自己的兄弟结构，兄弟节点的return指向同一个父节点
  index,
  ref, // ref属性

  pendingProps, // 即将更新的props
  memoizedProps, // 更新props
  memoizedState, // 当前状态
  dependencies, // 一个列表，存放这个Fiber依赖的context

  flags, // Effect，用来记录当前fiber的tag，插入、删除、更新的状态
  subtreeFlags,
  deletions,
  updateQueue, // 该Fiber对应的组件产生的Update会存放在这个队列里面
  nextEffect, // 单链表用来快速查找下一下side effect
  firstEffect, // 自述中第一个side effect
  lastEffect, // 子树中最后一个side effect

  lanes, // 当前Fiber更新的优先级
  childLanes,
  // 在Fiber树更新的过程中，每个Fiber都会有一个跟其对应的Fiber
  // 我们称他为`current <==> workInProgress`
  // 在渲染完成之后他们会交换位置
  alternate,

  // 用来描述当前Fiber和他子树的`Bitfield`
  // 共存的模式表示这个子树是否默认是异步渲染的
  // Fiber被创建的时候他会继承父Fiber
  // 其他的标识也可以在创建的时候被设置
  // 但是在创建之后不应该再被修改，特别是他的子Fiber创建之前
  mode,
  // 其余为调试相关的，收集每个Fiber和子树渲染时间的
  //...
}
```

### Hook 本质

- Hook 挂载在 Fiber 结点上的 memoizedState，为链式结构。
- 每当 Hook 函数被调用，react 就会创建一个 Hook 对象，并挂在链表的尾部，函数组件之所以能做一些类组件不能做的事儿，就是因为 Hook 对象，函数组件的状态，计算值，缓存等都是交给 Hook 去完成的，这样组件通过 Fiber.memoizedState 属性指向 Hook 链表的头部来关联 Hook 对象和当前组件，这样就发挥了 Hooks 的作用。每次调用 Hooks API 的时候，就会首先调用 createWorkInProgressHook 函数。得到 Hooks 的串联不是一个数组，而是一个链式结构，从根节点 workInProgressHook 向下通过 next 进行串联，这也是为什么 Hooks 不能嵌套使用，不能在条件判断中使用，不能在循环中使用，否则链式就会被破坏。

1. 状态 Hook(useState)

- 在状态 Hook 节点中，会通过 queue 属性上的循环链表记住所有的更新操作，并在 updade 阶段依次执行循环链表中的所有更新操作，最终拿到最新的 state 返回。

![截屏2022-05-10 下午4.49.55.png](/images/2022/05/10/V46eB9k753w2qmF.png)

2. 副作用 Hook (useEffect)

- 在副作用 Hook 节点中，创建 effect 挂载到 Hook 的 memoizedState 中，并添加到环形链表的末尾，该链表会保存到 Fiber 节点的 updateQueue 中，在 commit 阶段执行。

![截屏2022-05-10 下午4.50.05.png](/images/2022/05/10/hsVtjrKzxHGSdnX.png)

### Hook 基础使用

```js
import React, { useState } from 'react';
function App() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <div>{count}</div>
      <div
        onClick={() => {
          setCount(1);
          setCount((state) => count + 2);
          setCount((state) => count + 3);
        }}
      >
        加
      </div>
    </div>
  );
}
export default App;
```

1. 首次渲染主要是初始化 Hook，将初始值存入 Hook 内，将 Hook 插入到 fiber.memoizedState 的末尾。
2. setCount 主要是将更新信息插入到 hook.queue.penging 的末尾。这里注意一下为什么没有直接更新 hook.memoizedState 呢？答案是 react 是批量更新的，需要将更新信息先存储下来，等到合适的时机统一计算更新。
3. 再次渲染主要是根据 setCount 存储的更新信息来计算最新的 state。

### Hook 种类

* 所有的钩子都是为函数引入外部功能，react 约定，钩子一律使用 use 前缀命名。

| hook 功能类别  |         Hook         |                     具体功能                      | React v18新增 | 跨段是否支持(RN) |
| :------------: | :------------------: | :-----------------------------------------------: | :-----------: | :--------------: |
|  数据更新驱动  |       useState       |                   数据驱动更新                    |      否       |        是        |
|  数据更新驱动  |      useReducer      |          订阅状态，创建reducer，更新视图          |      否       |        是        |
|  数据更新驱动  | useSyncExternalStore |    concurrent模式下，订阅外部数据源，触发更新     |      是       |        否        |
|  数据更新驱动  |     usetranstion     |          concurrent模式下，过渡更新任务           |      是       |        否        |
|  数据更新驱动  |   useDeferredValue   |          concurrent模式下，更新状态滞后           |      是       |        否        |
|   执行副作用   |      useEffect       |        异步状态下，视图更新后，执行副作用         |      否       |        是        |
|   执行副作用   |   useLayoutEffect    |        同步状态下，视图更新前，执行副作用         |      否       |        是        |
|   执行副作用   |  useInetioonEffect   |            用于处理css in js 缺陷问题             |      是       |        否        |
| 状态获取和传递 |      useContext      | 订阅并获取react context上下文，用于跨层级状态传输 |      否       |        是        |
| 状态获取和传递 |        useRef        |               获取元素或者组件实例                |      否       |        是        |
| 状态获取和传递 | useImperativeHandle  |             用于函数组件能够被ref获取             |      否       |        是        |
| 状态派生和保存 |       useMemo        |           缓存状态，返回一个缓存的变量            |      否       |        是        |
| 状态派生和保存 |     useCallback      |           缓存状态，返回一个缓存的函数            |      否       |        是        |
|    工具hook    |        useID         |                    服务端渲染                     |      是       |        否        |
|    工具hook    |    useDebugValue     |                   devtool debug                   |      否       |        否        |

