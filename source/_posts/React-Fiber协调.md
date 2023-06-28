---
title: Fiber协调
date: 2020-6-19
tags: React
author: 映雪
---

相恨不如潮有信，相思始觉海非深。

<!--more-->

### Fiber 协调

-  react 更新，本质是 fiber tree 结构的更新变化。也就是 fiber 协调。

### React 协调模式

1. legacy mode 同步阻塞模式

2. Concurrent mode 并行模式

- 区别： 在于协调过程是否可中断

### Fiber 协调本质

1. 为workInProgress fiber tree 生成 fiber node
2. 找到发生的fiber node，更新fiber node，标记副作用effect
3. 收集带effect 的 fiber node 



