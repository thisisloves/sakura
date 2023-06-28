---
title: react-Fiber
date: 2020-4-19
tags: React
author: 映雪
---

泪眼问花花不语，乱红飞过秋千去。

<!--more-->

### React 核心思想

- 内存中维护一颗虚拟DOM树，数据变化时（setState），自动更新虚拟 DOM，得到一颗新树，然后 Diff 新老虚拟 DOM 树，找到有变化的部分，得到一个 Change(Patch)，将这个 Patch 加入队列，最终批量更新这些 Patch 到 DOM 中。

### React 工作过程

1. **调和阶段**(Reconciler)：React 会自顶向下通过递归，遍历新数据生成新的 Virtual DOM，然后通过 Diff 算法，找到需要变更的元素(Patch)，放到更新队列里面去。
2. **渲染阶段**(Renderer)：遍历更新队列，通过调用宿主环境的API，实际更新渲染对应元素。宿主环境，比如 DOM、Native、WebGL 等。

![截屏2023-05-12 上午11.33.30.png](/images/2023/05/12/9IqCUOBPWQobzhw.png)

- 在协调阶段阶段，由于是采用的递归的遍历方式，这种也被成为 Stack Reconciler，主要是为了区别 Fiber Reconciler 取的一个名字。这种方式有一个特点：一旦任务开始进行，就无法中断，那么 js 将一直占用主线程， 一直要等到整棵 Virtual DOM 树计算完成之后，才能把执行权交给渲染引擎，那么这就会导致一些用户交互、动画等任务无法立即得到处理，就会有卡顿，非常的影响用户体验。

### 合作式调度

- 把渲染更新过程拆分成多个子任务，每次只做一小部分，做完看是否还有剩余时间，如果有继续下一个任务；如果没有，挂起当前任务，将时间控制权交给主线程，等主线程不忙的时候在继续执行。 这种策略叫做 Cooperative Scheduling（合作式调度），操作系统常用任务调度策略之一。

### Fiber 由来

- React16 以前，对 virtural dom 的更新和渲染是同步的，大量的的同步计算任务阻塞了浏览器的 UI 渲染。默认情况下，JS 运算、页面布局和页面绘制都是运行在浏览器的主线程当中，他们之间是互斥的关系。如果 JS 运算持续占用主线程，页面就没法得到及时的更新。当我们调用 setState 更新页面的时候，React 会遍历应用的所有节点，计算出差异，然后再更新 UI。如果页面元素很多，整个过程占用的时机就可能超过 16 毫秒，就容易出现掉帧的现象。

![截屏2022-05-05 下午2.49.29.png](/images/2022/05/05/TxeDFdjNmXorywv.png)

### Fiber

- **fiber 把一个任务分成很多小片，当分配给这个小片的时间用尽的时候，就检查任务列表中有没有新的、优先级更高的任务，有就做这个新任务，没有就继续做原来的任务**。这种方式被叫做**时间分片异步渲染**(Async Rendering)。

![截屏2022-05-05 下午2.49.38.png](/images/2022/05/05/jGmO7u8PLhc6bRw.png)


### Fiber 概念

- **fiber 其实指的是一种数据结构**

![截屏2022-05-05 下午3.18.35.png](/images/2022/05/05/2WpDcFUbodgmlsi.png)

### Fiber 数据结构

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


### Fiber 实现原理

- react 内部运转分三层：

1. Virtual DOM 层，描述页面长什么样。
2. Reconciler 层，负责调用组件生命周期方法，进行 Diff 运算等。
3. Renderer 层，根据不同的平台，渲染出相应的页面，比较常见的是 ReactDOM 和 ReactNative。

### Fiber 任务优先级

- 为了实现不卡顿，就需要有一个调度器 (Scheduler) 来进行任务分配。优先级高的任务（如键盘输入）可以打断优先级低的任务（如 Diff）的执行，从而更快的生效。任务的优先级有六种：

1. synchronous，与之前的 Stack Reconciler 操作一样，同步执行
2. task，在 next tick 之前执行
3. animation，下一帧之前执行
4. high，在不久的将来立即执行
5. low，稍微延迟执行也没关系
6. offscreen，下一次 render 时或 scroll 时才执行

### Fiber 执行过程

1. 生成 Fiber 树，得出需要更新的节点信息。这一步是一个渐进的过程，可以被打断。阶段一可被打断的特性，让优先级更高的任务先执行，从框架层面大大降低了页面掉帧的概率。
2. 将需要更新的节点一次过批量更新，这个过程不能被打断。

- Fiber 树：React 在 render 第一次渲染时，会通过 React.createElement 创建一颗 Element 树，可以称之为 Virtual DOM Tree，由于要记录上下文信息，加入了 Fiber，每一个 Element 会对应一个 Fiber Node，将 Fiber Node 链接起来的结构成为 Fiber Tree。Fiber Tree 一个重要的特点是链表结构，将递归遍历编程循环遍历，然后配合 requestIdleCallback API, 实现任务拆分、中断与恢复。从 Stack Reconciler 到 Fiber Reconciler，源码层面其实就是干了一件递归改循环的事情。

### Fiber 工作流程

1. ReactDOM.render() 和 setState 的时候开始创建更新。
2. 将创建的更新加入任务队列，等待调度。
3. 在 requestIdleCallback 空闲时执行任务。
4. 从根节点开始遍历 Fiber Node，并且构建 WokeInProgress Tree。
5. 生成 **effectList**。
6. 根据 EffectList 更新 DOM。

![截屏2023-05-12 上午11.42.03.png](/images/2023/05/12/txRn4m5Fi3ZTLCv.png)

1. 第一部分从 ReactDOM.render() 方法开始，把接收的 React Element 转换为 Fiber 节点，并为其设置优先级，创建 Update，加入到更新队列，这部分主要是做一些初始数据的准备。
2. 第二部分主要是三个函数：scheduleWork、requestWork、performWork，即安排工作、申请工作、正式工作三部曲，React 16 新增的异步调用的功能则在这部分实现，这部分就是 Schedule 阶段，前面介绍的 Cooperative Scheduling 就是在这个阶段，只有在这个解决获取到可执行的时间片，第三部分才会继续执行。具体是如何调度的，后面文章再介绍，这是 React 调度的关键过程。
3. 第三部分是一个大循环，遍历所有的 Fiber 节点，通过 Diff 算法计算所有更新工作，产出 EffectList 给到 commit 阶段使用，这部分的核心是 beginWork 函数，这部分基本就是 Fiber Reconciler ，包括 reconciliation 和 commit 阶段。


### Fiber 影响

- 由于任务执行过程可以被打断，这几个生命周期 componentWillMount componentWillReceiveProps componentWillUpdate 可能会执行多次，如果它们包含副作用（比如 AJax），会有意想不到的 bug。EffectList

### 补充

- React 使用“双缓存”来完成 Fiber 树的构建与替换 ，对应着 DOM 树的创建与更新。双缓存是一种在内存中构建并直接替换的技术。