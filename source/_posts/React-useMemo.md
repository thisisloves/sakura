---
title: useMemo
date: 2020-6-12
tags: React
author: 映雪
---

听闻远方有你，动身跋涉千里，我吹过你吹过的风，这算不算相拥，我踏过你走过的路，这算不算相逢，我还是很喜欢你，认真且怂，从一而终。

<!--more-->

### useMemo

- useMemo用来做缓存用的，只有当一个依赖项改变的时候才会发生变化，否则拿缓存的值，就不用在每次渲染的时候再做计算。

### React 更新机制

1. 更新虚拟 dom
2. 如果更新后的虚拟 dom 和老的虚拟 dom 有差异，则更新真实 dom，否则不更新真实 dom


### useMemo 语法

```js
useMemo(() => {}, []);
```

- useMemo 接收两个参数，分别是函数和一个数组（实际上是依赖），返回缓存的变量,数组内存放依赖。

### useMemo 和 useEffect 对比

```jsx
import React, { useEffect, useMemo, useState } from 'react';
import Son from './Son';

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

  const newState = useMemo(()=>{
    console.log("useMemo")
    return state
  },[state.m])


  useEffect(()=>{
    console.log("useEffect")
  },[state.m])
  
  return (
      <div>
       <button onClick={addN}>设置n</button>
      <button onClick={addM}>设置m</button>
      <Son state={newState}></Son>
      </div>
  );
  }
export default App;

```

- useMemo是在useEffect前面的，useEffect可看成class组件里面的componentDidMount，componentDidUpdate 和 componentWillUnmount 这三个函数的组合。 而useMemo则可以看作是componentWillUpdate函数。


![截屏2023-05-19 下午4.13.53.png](/images/2023/05/19/WCkmqjwHyM69r8u.png)

- 区别:**useEffect没有返回值（在不考虑解绑的情况下），并且是在页面渲染之后才执行的，而useMemo有返回值，并且是在页面渲染的时候进行的**

### useMemo 使用

1. **子组件使用memo，父组件传递对象，子组件不使用useMemo**

```jsx
import React, { useEffect,memo, useMemo, useState } from 'react';


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
  
  const Son = memo(({state}) => {
    console.log("子组件触发了喔")
 
   return (
     <div>
       儿子组件的m  :{state.m}
     </div>
   );
 });
 
  return (
      <div>
       <button onClick={addN}>设置n</button>
      <button onClick={addM}>设置m</button>
      <Son state={state}></Son>
      </div>
  );
  }
export default App;

```

- 在这场景下，子组件memo无法对对象进行浅比较，当父组件的n变化，子组件仍然会render，实际上希望父组件m变化才重新render，n变化不渲染

2. **子组件使用memo，父组件传递对象，子组件使用useMemo**

```jsx
import React, { useEffect,memo, useMemo, useState } from 'react';


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

  const newState = useMemo(()=>{
    return state
  },[state.m])

  
  const Son = memo(({state}) => {
    console.log("子组件触发了喔")
 
   return (
     <div>
       儿子组件的m  :state.m
     </div>
   );
 });
 
  return (
      <div>
       <button onClick={addN}>设置n</button>
      <button onClick={addM}>设置m</button>
      <Son state={newState}></Son>
      </div>
  );
  }
export default App;

```

- 在这场景下，子组件memo无法对对象进行浅比较，但是当父组件的n变化，子组件state没有变化，就实现了父组件m变化才子组件才render。



### useMemo 应用场景

1. 登陆之后，你的个人信息一般是不会变的，当你退出登陆，重新输入另外一个人的账号密码之后，这个时候个人信息可能就变了，那这样我就可以把账号和密码`两个作为依赖项，当他们变了，那就更新个人信息，否则拿缓存的值，从而达到优化的目的。

2. A页面跳转到B页面，并携带一些参数，可从路由下手把路由参数当**依赖项**，只有依赖变了，才会变化数据。

3. **需要大量计算的函数，或者引用类型的变量。**

```jsx

import React, { useEffect,memo, useMemo, useState } from 'react';


const App = () => {
  // 调用这个函数需要大量时间去计算
  const slowFunction = (number) => {
      console.log('calling slow function')
      for (let i = 0; i <= 1000000000; i++) {
      }
      return number * 2
  }
  const [inputNumber, setInputNumber] = useState(1)
  const [dark, setDark] = useState(true)

  // 场景1:执行某函数需要大量时间，使用useMemo来优化，在不必要执行函数的时候不执行函数
  const doubleNumber = useMemo(() => {
      return slowFunction(inputNumber)
  }, [inputNumber])

  // 场景2:每次组件更新会重新执行，内部的引用类型变量会重新创建，这会导致使用到引用类型变量的组件重新渲染，使用useMemo来让每次的变量相同
  const themeStyle = useMemo(() => {
      return {
          background: dark ? 'black' : 'white',
          color: dark ? 'white' : 'black'
      }
  }, [dark])

  useEffect(() => {
      console.log('themeStyle changed')
  }, [themeStyle])

  const handleChange = (e) => {
      setInputNumber(parseInt(e.target.value))
  }
  return <>
          <input type='text' value={inputNumber} onChange={handleChange}/>
          <button onClick={() => {
              setDark(prevDark => !prevDark)
          }}>change theme
          </button>
          <p style={themeStyle}>{doubleNumber}</p>
      </>
  
}

export default App;
```