---
title: JSX
date: 2019-3-20
tags: React
author: 映雪
---

曾经沧海难为水，除却巫山不是云。

<!--more-->

### JSX

- JSX 是 JavaScript 的一种语法扩展，它和模板语言很接近，但是它充分具备 JavaScript 的能力。

### JSX 语法是如何在 JavaScript 中生效的？

- 因为 JSX 是 JS 的扩展，所以浏览器并不会像天然支持 JavaScript 支持 JSX，所以我们需要用到 Babel 来解析。 JSX 会被编译为 React.createElement(),React.creacteElement()将会返回一个叫做“React Element(虚拟 DOM)”的 JS 对象。

![截屏2022-04-29 下午5.42.11.png](/images/2022/04/29/WechatIMG6.jpeg)

### JSX 的本质

- jsx 仅仅只是 React.createElement(component, props, ...children)函数的语法糖
- 所有的 jsx 最终都会被转换成 React.createElement 的函数调用
- jsx -> babel -> React.createElement()

### Babel

- Babel 是一个工具链，主要用于 ECMAScript2015+版本的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前环境和旧版本的浏览器或其它环境中。

### 虚拟 DOM 是如何渲染成真实 DOM 的？

- 通过 ReactDOM.render()

```jsx
ReactDOM.render(
  //需要渲染的元素
  element,
  //元素挂载的目标容器，一个真实DOM
  container,
  //回调函数，可选，用来处理渲染结束后的逻辑
  callback,
);
```

![截屏2022-04-29 下午5.48.35.png](/images/2022/04/29/WechatIMG8.jpeg)
