---
title: React.memo
date: 2020-6-11
tags: React
author: 映雪
---

春城无处不飞花，寒食东风御柳斜。

<!--more-->

### React.memo

- React 顶层 Api 方法，给函数组件来减少重复渲染，类似于 PureComponent 和 **shouldComponentUpdate(类组件优化)** 方法的集合体。
- React.memo顶层Api方法，它可以用来减少子组件的重复渲染次数，从而提升组件渲染性能。
- React.memo它是一个只能在函数组件中使用的顶层Api方法。

### React.memo 使用原因

- 当父组件发生改变时，默认情况下它的子孙组件也会重新渲染，当某些子组件不需要更新时，也会被强制更新，对于没有用到被改变的那个状态的组件来说，重新渲染是完全没有必要的，为了避免这种情况，React.memo就诞生了。

### React.memo 用法

```jsx
React.memo(component, Function)
```

1. 第一个参数是自定义组件
2. 第二个参数是一个函数，用来判断组件需不需要重新渲染。如果省略第二个参数，默认会对该组件的props进行浅比较


### React.memo 使用

1. **memo 不使用，同时子组件即使和父组件没有任何关联**

```jsx
import React, { useState } from 'react';

const Son2 = () => {
   console.log("子组件1触发了")
  return (
    <div>
      儿子组件1 的m  :
    </div>
  );
};

const Son = (props) => {
   const { state, setState } = props;

 
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
      儿子组件2
      <div>m:{state.m}</div>
      <div>n:{state.n}</div>
      <button onClick={addN}>设置n</button>
      <button onClick={addM}>设置m</button>
    </div>
  );
};

const App = ()=>{
  const [state, setState] = useState({ m: 100, n: 50 });

  return (
      <div>
      <Son></Son>
      <Son2 state={state} setState={setState}></Son2>
      </div>
  );
  }
export default App;


```

- 在不使用 React.memo 方法的情况下，子组件即使和父组件没有任何关联，只要父组件更新，子组件也会跟着更新


2. **memo 使用,同时子组件即使和父组件没有任何关联**

```jsx
import React, { useState ，memo} from 'react';

const Son2 = memo(() => {
   console.log("子组件1触发了")
  return (
    <div>
      儿子组件1 的m  :
    </div>
  );
});

const Son = (props) => {
   const { state, setState } = props;

 
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
      儿子组件2
      <div>m:{state.m}</div>
      <div>n:{state.n}</div>
      <button onClick={addN}>设置n</button>
      <button onClick={addM}>设置m</button>
    </div>
  );
};

const App = ()=>{
  const [state, setState] = useState({ m: 100, n: 50 });

  return (
      <div>
      <Son></Son>
      <Son2 state={state} setState={setState}></Son2>
      </div>
  );
  }
export default App;


```

 - 当父组件变化，子组件即使和父组件没有任何关联，使用memo子组件不更新 


3. **父组件向子组件传值，memo 使用情况**

```jsx
import React, { useState ，memo} from 'react';

const Son2 = memo(({m}) => {
  console.log(m)

   console.log("子组件1触发了喔")

  return (
    <div>
      儿子组件1 的m  :
    </div>
  );
});

const Son = (props) => {
   const { state, setState } = props;

 
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
      儿子组件2
      <div>m:{state.m}</div>
      <div>n:{state.n}</div>
      <button onClick={addN}>设置n</button>
      <button onClick={addM}>设置m</button>
    </div>
  );
};

const App = ()=>{
  const [state, setState] = useState({ m: 100, n: 50 });

  return (
      <div>
      <Son m={state.m}></Son>
      <Son2 state={state} setState={setState}></Son2>
      </div>
  );
  }
export default App;

```

-  当父组件变化，子组件和父组件传值关联，只有传值变化时才会触发子组件更新

4. **父组件向子组件传值为对象时，memo 使用情况**


```jsx
import React, { useState ，memo} from 'react';

const Son2 = memo(({state}) => {
  console.log(state)

   console.log("子组件1触发了喔")

  return (
    <div>
      儿子组件1 的m  :
    </div>
  );
},  (prevProps, nextProps) => {
  console.log(prevProps, nextProps)
  return prevProps.state.m === nextProps.state.m
}
)

const Son = (props) => {
   const { state, setState } = props;

 
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
      儿子组件2
      <div>m:{state.m}</div>
      <div>n:{state.n}</div>
      <button onClick={addN}>设置n</button>
      <button onClick={addM}>设置m</button>
    </div>
  );
};

const App = ()=>{
  const [state, setState] = useState({ m: 100, n: 50 });

  return (
      <div>
      <Son state={state}></Son>
      <Son2 state={state} setState={setState}></Son2>
      </div>
  );
  }
export default App;

```

- **React.memo参数2返回值如果为true则表示停止渲染，false继续渲染**
- 参数2写的回调函数，一般情况下都在props传过来的数据为引用类型，才需要手动来判断，如果是基本类型则不需要写参数2，来手动判断。
- 传递参数为对象时，通过React.memo第二个参数返回值判断是否触发更新。


### React.memo 思考

- 用memo或者useMemo做优化时，如果你可以从不变的部分里**分割出变化**的部分，那么这看起来可能是有意义的。
- 用memo会有性能问题，比如首次渲染、重新渲染、多次渲染场景下，memo有可能会有性能提升，也有可能会有性能损耗。
- **缓存本身也是需要成本的**，如果这个成本大于它所带来的收益，但就是负收益了，就变成无用了。


### useMemo和useCallback 性能优化组合

1. useMemo

- 第一次渲染时执行，缓存变量，之后只有在依赖项改变时才会重新计算记忆值

2. useCallback

- 第一次渲染时执行，缓存函数，之后只有在依赖项改变时才会更新缓存



```jsx
import React, { useState ，memo} from 'react';
const Son = memo(({state}) => {
   console.log("子组件1触发了喔")
  return (
    <div>
      儿子组件
    </div>
  );
}
)

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
  return (
      <div>
      <button onClick={addN}>设置n</button>
      <button onClick={addM}>设置m</button>
      <Son state={state} setState={setState}></Son>
      </div>
  );
  }
export default App;

```

- 每次父组件更新的时候，子组件的state和setState值都会**生成新的内存地址**,子组件使用memo仍然更新。


```jsx
import React, { useState,memo,useMemo,useCallback} from 'react';
const Son = memo(({state}) => {
   console.log("子组件1触发了喔")
  return (
    <div>
      儿子组件
    </div>
  );
}
)

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

    const onChangeState = useCallback((m)=>{
    setState((state) => {
      return { ...state, m: state.m + 1 };
    });
  },[state.m]) 

  const stateNew = useMemo(()=>state
  ,[state.m])

  return (
      <div>
      <button onClick={addN}>设置n</button>
      <button onClick={addM}>设置m</button>
      <Son state={stateNew} setState={onChangeState}></Son>
      </div>
  );
  }
export default App;

```


- 使用useMemo 和useCallback 缓存变量和函数 ，若依赖不改变则内存地址不会改变，相当子组件和父组件没有联系，则子组件memo生效。