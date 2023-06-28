---
title: React.Suspense
date: 2020-6-17
tags: React
author: 映雪
---

鱼沈雁杳天涯路,始信人间别离苦。

<!--more-->

### Code Spliting 

- 在 16.6 版本之前，code-spliting 通常是由第三方库来完成的，比如 react-loadble(核心思路为: 高阶组件 + webpack dynamic import), 在 16.6 版本中提供了 Suspense 和 lazy 这两个钩子, 因此在之后的版本中便可以使用其来实现 Code Spliting。 

```jsx
import { Suspense, lazy } from 'react'

const DemoA = lazy(() => import('./demo/a'))
const DemoB = lazy(() => import('./demo/b'))

<Suspense>
  <NavLink to="/demoA">DemoA</NavLink>
  <NavLink to="/demoB">DemoB</NavLink>

  <Router>
    <DemoA path="/demoA" />
    <DemoB path="/demoB" />
  </Router>
</Suspense>
```

### 动态import分割代码

```jsx
const mathAdd = () => {
   import("./math").then(math => {
     console.log(math.add(16, 26));
  });
  }
```

- import直接使用，但是要注意返回的是一个promise对象，所以需要使用.then接收
- 动态 import() 语法，该语法返回一个 Promise，当 Webpack 解析到该语法时，会自动进行代码分割。


### React.Suspense

- Suspense 组件，让你可以 "等候" 目标代码加载，而且可以直接指定一个加载的界面（像是个 spinner），让它在用户等候的时分显现。
- Suspense 主要是为了解决两个问题

1. 代码分割
2. 数据获取

- React.Suspense单独使用

```jsx
let data, promise;
function fetchData() {
  if (data) return data;
  promise = new Promise(resolve => {
    setTimeout(() => {
      data = 'data fetched'
      resolve()
    }, 3000)
  })
  throw promise;
}
function Content() {
  const data = fetchData();
  return <p>{data}</p>
}
function App() {
  return (
    <Suspense fallback={'loading data'}>
      <Content />
    </Suspense>
  )
}
```

- <Content> 组件会 throw 一个 promise，React 会捕获这个反常，发现是 promise 后，会在这个 promise 上追加一个 then 函数，在 then 函数中履行 Suspense 组件的更新，然后展现 fallback 内容。

- fetchData 中的 promise resolve 后，会履行追加的 then 函数，触发 Suspense 组件的更新，此时有了 data 数据，由于没有反常，React 会删去 fallback 组件，正常展现 <Content /> 组件。