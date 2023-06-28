---
title: React父组件调用子组件方法
date: 2019-6-11
tags: React
author: 映雪
---

曾经拥有的，不要忘记。不能得到的，更要珍惜。属于自己的，不要放弃。已经失去的，留作回忆。

<!--more-->

### class 组件父组件调用子组件方法

1. 给子组件添加事件属性

```js
import React from 'react';
import styles from './index.less';
import ListComponent from '@/components/List/index.jsx';
import { Button } from 'antd';

class Home extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  }
  render() {
    return (
      <div className={styles.home}>
        <ListComponent onRef={(ref) => (this.child = ref)} />
        <Button onClick={() => this.child.functionChild()}> 调用子组件的方法</Button>
      </div>
    );
  }
}

export default Home;
```

2. 子组件内调用 onRef

```js
import { Button } from 'antd';
import React from 'react';

class List extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  }

  componentDidMount() {
    if (this.props.onRef) {
      this.props.onRef(this);
    }
  }
  functionChild() {
    console.log('这是子组件的方法');
  }
  render() {
    return <div></div>;
  }
}

export default List;
```

- 注意：即使父组件引用了 child.state 中的数据，子组件 state 的更新也不会引起父组件的重新渲染

### 函数组件父组件调用子组件方法

- 函数组件要使用 useImperativeHandle 和 forwardRef

```js
import React, { useRef } from 'react';
import TabComponent from '@/components/Tab/index.jsx';
import { Button } from 'antd';

const Fun = (props) => {
  const childRef = useRef();

  return (
    <div>
      <TabComponent ref={childRef} />
      <Button onClick={() => childRef.current.funChild()}>这是子的方法</Button>
    </div>
  );
};

export default Fun;

```

- 子组件通过useImperativeHandle 和 forwardRef 暴露方法变量。

```js
import React, { useState, forwardRef, useImperativeHandle } from 'react';

const Tab = (props, ref) => {

  useImperativeHandle(ref, () => ({
    funChild,
  }));

  const funChild = ()=>{
    console.log('这是子的方法')
  }

  return <div></div>;
};

export default forwardRef(Tab);
```