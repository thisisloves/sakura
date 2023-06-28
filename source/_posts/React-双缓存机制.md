---
title: 双缓存机制
date: 2020-4-20
tags: React
author: 映雪
---

岁月不堪数，故人不如初。

<!--more-->

### 双缓存

- 以 canvas 为例子，当我们用canvas绘制动画，每一帧绘制前都会调用ctx.clearRect清除上一帧的画面。
- 如果当前帧画面计算量比较大，我们就很容易因为绘制太慢了出现白屏。
- 为了解决这个问题，我们可以先在内存中绘制当前帧的动画，绘制完毕后，直接用当前帧替换上一帧，这样就省去了两帧的替换时间，不会出现从白屏到出现画面的闪烁情况。
- React使用“双缓存”来完成Fiber树的构建与替换——对应着DOM树的创建与更新。

### Fiber

- Fiber 是一个分片执行单元，每执行完一个单元，React会检查现在还剩多少时间，如果没有时间则交出控制权。
- Fiber是一个链表数据结构：

1. Child 指向 当前Fiber的第一个子节点
2. Sibling 指向当前Fiber的的下一个兄弟节点
3. Return 指向当前 Fiber 节点的 父节点

- 由于要实现低优先级任务的暂停更新，但是在高优先级任务执行完成之后还要回来继续执行低优先级任务，因此使用链表数据结构，通过指针和地址对任务进行关联。

### Fiber 使用原因

- 当 React 树太过庞大时，递归Diff会很消耗时间，会造成主线程被持续占用，有一些高优先级的任务得不到处理。因为JS单线程的原因，每个同步任务不能阻塞太长时间，因此需要对一些耗时长的任务进行分片，每一个分片所对应的数据结构就是Fiber。

- 高优先级任务：例如用户交互，点击或输入事件。如果点击或输入迟迟无法得到响应，用户体检一定是极差的。

### React 双缓存

- react 根据双缓冲机制维护了两个fiber树，一颗用于渲染页面 (current)，一颗是 workInProgress Fiber 树，用于在内存中构建，然后方便在构建完成后直接昔换 current Fiber树
- workInprogress Fiber 树的 alternate 指向 Current Fiber树的对应节点, current 表示页面正在使用的 fiber 树
- 当 workInprogress Fiber 树构建完成，**workInProgress Fiber 则成为了 current 渲染到页面上**，而之前的 **current 则缓存起来成为下一次的workInProgress Fiber**，完成双缓冲模型

### 双缓存模型

![截屏2023-05-12 下午1.46.26.png](/images/2023/05/12/be6o1W2hBQdkVtA.png)

```html
<div key="A">
  <div key="B1"></div>
  <div key="B2"></div>
</div>
```

- A的 child 指向第一个儿子 B1，B1的 return 指向 A
- B1的 sibiling指向他的兄弟 B2，B2的 return 指向 A

### 双缓存理解

- 假如你去看一场总时长只有 1 个小时的话剧，这场话剧中场不休息，需要不间断地演出。照剧情的需求，半个小时处需要一次转场。所谓转场，就是说话剧舞台的灯光、布景、氛围等全部要切换到另一种风格里去。在不中断演出的情况下，想要实现转场，怎么办呢？场务工作做得再快，也要十几二十分钟，这对一场时长 1 小时的话剧来说，实在太漫长了。观众也无法接受这样的剧情"卡顿"体验。

- 解法：准备两个舞台来做这场戏，当第一个舞台处于使用中时，第二个舞台的布局已经完成。这样当第一个舞台的表演结束时，只需要把第一个舞台的灯光灭掉，第二个舞台的灯光亮起，就可以做到剧情的无缝衔接了。

- 在这个过程中，我们可以认为，左侧舞台和右侧舞台分别是两套缓冲数据，而呈现在观众眼前的连贯画面，就是不同的缓冲数据交替被读取后的结果。

### 双缓存实例

```js
function App() {
  const [num, add] = useState(0);
  return (
    <p onClick={() => add(num + 1)}>{num}</p>
  )
}

ReactDOM.render(<App/>, document.getElementById('root'));
```

1. **mount**

- 由于是首次渲染，页面中还没有挂载任何DOM，所以fiberRootNode.current指向的rootFiber没有任何子Fiber节点（即current Fiber树为空）。

![截屏2023-05-12 下午2.01.22.png](/images/2023/05/12/OQMsWtnvS1fTA8F.png)

2. **render** 

- 根据组件返回的JSX在内存中依次创建Fiber节点并连接在一起构建Fiber树，被称为workInProgress Fiber树。
- 在构建workInProgress Fiber树时会**尝试复用current Fiber树中已有的Fiber节点内的属性**，在首屏渲染时只有rootFiber存在对应的current fiber（即rootFiber.alternate）


![截屏2023-05-12 下午2.01.37.png](/images/2023/05/12/UnqYwgLiWfdJvCN.png)

3. **commit**

- 已构建完的workInProgress Fiber树在commit阶段渲染到页面。fiberRootNode的current指针指向workInProgress Fiber树使其变为current Fiber 树。

![截屏2023-05-12 下午2.01.37.png]( /images/2023/05/12/zYL7AW52FB9p8hs.png)

3. **udpate**  

- 接下来我们点击p节点触发状态改变，这会开启一次新的render阶段并构建一棵新的workInProgress Fiber 树。在构建workInProgress Fiber树时会尝试复用current Fiber树中已有的Fiber节点内的属性

![截屏2023-05-12 下午2.13.12.png](/images/2023/05/12/N9alyEv4iFgCDeA.png)

5. **commit** 

- render阶段完成构建后进入commit阶段渲染到页面上。渲染完毕后，使用 workInProgress Fiber 替换 current Fiber 树。

![截屏2023-05-12 下午2.01.37.png](/images/2023/05/12/djhXwRQTtcGvgV7.png)
