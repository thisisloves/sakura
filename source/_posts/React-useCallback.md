---
title: useCallback
date: 2020-6-13
tags: React
author: 映雪
---

最是凝眸无限意，似曾相逢在前生。

<!--more-->

### useCallback

```js
useCallback(fn, deps) 
// 等价
useMemo(() => fn, deps)
```

### React 更新机制

1. 更新虚拟 dom
2. 如果更新后的虚拟 dom 和老的虚拟 dom 有差异，则更新真实 dom，否则不更新真实 dom


### useCallback 语法

```js
useCallback(() => {}, []);
```

- useCallback 接收两个参数，分别是函数和一个数组（实际上是依赖），返回缓存的函数,数组内存放依赖。
- 会在渲染期间执行，返回一个函数，useCallback是用来帮忙缓存函数的，当依赖项没有发生变化时，返回缓存的指针，当在依赖项变化的时候会更新，返回一个新的函数


### 与 useEffect的执行时机对比

- useMemo和useCallback都会在组件第一次渲染的时候执行，之后会在其依赖的变量发生改变时再次执行；并且这两个hooks都返回缓存的值，useMemo返回缓存的变量，useCallback返回缓存的函数。


### useCallback 使用

```jsx
import React, { useEffect,memo, useMemo, useState, useCallback } from 'react';

const Son = memo((props) => {
  console.log("子组件触发了喔")
 return (
   <div>
    
     儿子组件
   </div>
 );
});

const App = ()=>{
  const [state, setState] = useState({ m: 100, n: 50 });
  
  const addN = () => {
    setState((state) => {
      return { ...state, n: state.n + 1 };
    });
  };
  const addM = () => {
    setState((state) => {
      return { ...state, m: state.m + 1 };
    });
  };

  const callbackAdd = useCallback(()=>{
    setState((state) => {
      return { ...state, m: state.m + 1 };
    });
  },[state.m])
  
  return (
      <div>
       <button onClick={addN}>设置n</button>
      <button onClick={addM}>设置m</button>
      <Son  callbackAdd={callbackAdd}></Son>
      </div>
  );
  }
export default App;

```

- 父组件传递方法，子组件memo 浅比较，若不使用useCallback 则改变n也会触发子组件渲染

### useCallback 理解

1. callbackFunc函数每次都会新建，useCallback不是为了减少函数创建次数

```jsx
const DemoComponent = ()=>{ 
    // 先创建了一个函数 funB
    const callbackFunc = () => {};
    // 函数 funB 当作第一个参数传入 useCallback 中;deps为[]
    const funA = useCallback(callbackFunc, []);
    // 省略代码...
}

```

2. 不要所有函数包一层useCallback，useCallBack会造成多余函数执行，在大量计算的函数中使用


```jsx
import React, { useState, useCallback,memo } from "react";

const SonExpenceComponent = memo(({addCount} ) => (
 
  <div>
   { console.log("子组件渲染了")}
    <button onClick={addCount}>Add</button>
    {new Array(1000).fill("item").map((i, index) => (
      <div>{i + index}</div>
    ))}
  </div>
));

const App = () => {
  const [count, setCount] = useState(0);

  const addCount = useCallback(() => {
    console.log(count)
    setCount((count) => count + 1);
  }, []);

  return (
    <>
      <p>Count: {count}</p>
      <SonExpenceComponent addCount={addCount} />
    </>
  );
};

export default App

```
- 子组件大数据渲染只渲染一次，优化了性能

###  useCallback 案例

- 存在问题：每次count 变化都会引起子组件更新

```jsx
import { useState, useCallback, memo } from "react";

const SonExpenceComponent = memo(({ handleClick }) => {
  return <button onClick={handleClick}>Click!</button>;
});

function ParentComponent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  return (
    <div>
      <p>{count}</p>
      <SonExpenceComponent handleClick={handleClick} />
    </div>
  );
}

export default ParentComponent;
```

1. 使用setCount 获取最新的count 

```jsx
import { useState, useCallback, memo } from "react";

const SonExpenceComponent = memo(({ handleClick }) => {
  return <button onClick={handleClick}>Click!</button>;
});

function ParentComponent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(count => count + 1);
  }, []);

  return (
    <div>
      <p>{count}</p>
      <SonExpenceComponent handleClick={handleClick} />
    </div>
  );
}

export default ParentComponent;
```

2. 官方解法，使用 useEffect 和 useRef 结合，useRef 获取最新count

```jsx
import { useState, useCallback, useRef, useEffect, memo } from "react";

const SonExpenceComponent = memo(({ handleClick }) => {
  return <button onClick={handleClick}>Click!</button>;
});

function ParentComponent() {
  const [count, setCount] = useState(0);
  const countRef = useRef(0);

  const handleClick = useCallback(() => {
    setCount(countRef.current);
  }, []);
  
  // 同步countRef
  useEffect(() => {
    countRef.current = count;
  }, [count]);

  return (
    <div>
      <p>{count}</p>
      <SonExpenceComponent handleClick={handleClick} />
    </div>
  );
}

export default ParentComponent;

```

3. ahooks 的 useMemoizeFn

```jsx
import { useMemo, useRef } from 'react';

type noop = (this: any, ...args: any[]) => any;

type PickFunction<T extends noop> = (
  this: ThisParameterType<T>,
  ...args: Parameters<T>
) => ReturnType<T>;

function useMemoizedFn<T extends noop>(fn: T) {
  const fnRef = useRef<T>(fn);
  // 永远拿到最新fn
  fnRef.current = useMemo(() => fn, [fn]);

  const memoizedFn = useRef<PickFunction<T>>();
  if (!memoizedFn.current) {
    memoizedFn.current = function (this, ...args) {
      return fnRef.current.apply(this, args);
    };
  }

  return memoizedFn.current;
}

export default useMemoizedFn;
```