---
title: useContext
date: 2020-6-9
tags: React
author: 映雪
---

时有落花至，远随流水香。

<!--more-->

### useContext

- 在hooks之前，react开发者都是使用class组件进行开发，父子组件之间通过props传值。但是现在开始使用方法组件开发，没有constructor构造函数，也就没有了props的接收，所以父子组件的传值就成了一个问题。
- useContext可以看成是扩大版的 props，它可以将全局的数据通过 provider 接口传递 value 给局部的组件，让包围在 provider 中的局部组件可以获取到全局数据的读写接口。

### useContext 作用

1. useContext可以帮助我们跨越组件层级直接传递变量，实现数据共享。
2. Context的作用就是对它所包含的组件树提供全局共享数据的一种技术。

### useContext 用法

1. 创建 context
2. 设置 provider 并通过 value 接口传递 state
3. 局部组件获取读写接口

### useContext 使用

- 创建 Context ,设置 provider 并通过 value 接口传递 state 数据

```js
import React, { useRef, createContext, useState } from 'react';
import Son from './Son';

const Context = createContext(); // 创建Context
let a = 0;

const UseContext = (props) => {
  console.log(`render了${a}次`);
  const [state, setState] = useState({ m: 100, n: 50 });
  a += 1;

  return (
    // 通过provider提供value给包围里内部组件，只有包围里的组件才有效
    <Context.Provider value={{ state, setState }}>
      m:{state.m}，n:{state.n}
      <Son Context={Context}></Son>
    </Context.Provider>
  );
};
export default UseContext;
```

- 局部组件从 value 接口中传递的数据对象中获取读写接口

```js
import React, { createContext, useContext, useState } from 'react';

const Son = (props) => {
  //拿到props.Context
  const { state, setState } = useContext(props.Context);

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
  return (
    <div>
      儿子组件
      <div>n:{state.n}</div>
      <button onClick={addN}>设置n</button>
      <button onClick={addM}>设置m</button>
    </div>
  );
};

export default Son;
```

### useContext 案例(实现redux效果,多级数据传输，全局数据共享) 

1. 创建context.js，存储数据

```js
import React from "react"

const cityContext = React.createContext({
    city:["广州"],
    number:11,
})
export {
    cityContext
}
```

2. 父级引用

```jsx
import React, {  useCallback, useEffect, useMemo, useState } from 'react';
import {cityContext} from './context';
import Son from './Son';


const Father = () => {
  const [number,setNumber] = useState(111)
  const addNumber = useCallback(()=>{setNumber((number)=>number+1)},[])
  let city = ["北京","上海","深圳"]
  
  return (
    // 通过provider提供value给包围里内部组件，只有包围里的组件才有效
    <cityContext.Provider value={{ number,addNumber,city }}>
     <div style={{width:'500px',height:'500px',border:"1px solid green"}}>
      父组件<br/>
      city:{city}<br/>
      number:{number}
      <Son ></Son>
      </div>
    </cityContext.Provider>
  );
};
export default Father;

```

3. 子级修改

```jsx
import React, { memo,createContext, useContext, useState } from "react";
import {cityContext} from './context';
import Sun from './Sun';

const Son = (props) => {
  const {city ,number,addNumber} =  useContext(cityContext)
  return (
    <div style={{width:'400px',height:'400px',border:'1px solid red'}}>
      儿子组件<br/>
      city:{city}<br/>
      number:{number}<br/>
      <button onClick={() => {
              addNumber()}}>add+1
          </button>
     {/* 孙子组件 */}
    <Sun></Sun>
    </div>
  );
};

export default Son

```


4. 孙子级引用

```jsx
import React,{useContext, useState} from "react";
import {cityContext} from './context';

const Sun = (props) => {
  const {city ,number} =  useContext(cityContext)

  return (
    <div style={{width:'300px',height:'300px',border:'1px solid #000'}}>
      孙子组件<br/>
      city:{city}<br/>
      number:{number}<br/>
    </div>
  );
};

export default Sun

```

![截屏2023-05-30 上午8.54.46.png](/images/2023/05/30/XdAmBz8ph5OI3LR.png)

- 当子组件调用添加number方法 ，父组件以及孙子组件的number 同时变化

