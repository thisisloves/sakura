---
title: useImperativeHandle
date: 2020-6-14
tags: React
author: 映雪
---

何如薄幸锦衣郎，比翼连枝当日愿。

<!--more-->

### useImperativeHandle

- 减少暴露给父组件获取的DOM元素属性, 只暴露给父组件需要用到的DOM方法，useImperativeHandle应当与forwardRef一起使用。

### useImperativeHandle 语法

```jsx
useImperativeHandle(ref, createHandle, [deps])
```

1. 参数1: 父组件传递的ref属性
2. 参数2: 返回一个对象，以供给父组件中通过ref.current调用该对象中的方法。
3. 参数3: 依赖项数组，当依赖项发生变化时，会重新调用回调函数。

### useImperativeHandle 使用

```jsx
import { useImperativeHandle, forwardRef, useState } from 'react';

const ChildComponent = forwardRef((props, ref) => {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(prevCount => prevCount + 1);
  };

  // 动态修改暴露的方法
  useImperativeHandle(ref, () => ({
    increment
  }), [count]);

  return (
    <div>
      Child Component Count: {count}
    </div>
  );
});

// 在父组件中使用
const ParentComponent = () => {
  const childRef = useRef();

  const handleClick = () => {
    childRef.current.increment();
  };

  return (
    <div>
      <ChildComponent ref={childRef} />
      <button onClick={handleClick}>Increment Child's Count</button>
    </div>
  );
};
```

### useImperativeHandle 原理

- useImperativeHandle的原理是通过将子组件内部定义的方法和属性绑定到ref对象上，从而使父组件可以通过ref.current访问和调用子组件的方法和属性。这样就可以实现子组件向父组件暴露指定接口的目的，从而实现组件之间的通信和数据传递。

