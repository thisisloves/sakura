---
title: React组件
date: 2019-2-6
tags: React
author: 映雪
---

愿将你葬此，三生成牵挂。

<!--more-->

### 组件

- 组件是对数据和方法的简单封装。

### 组件规则

1. props 的只读性，组件无论是使用函数声明还是通过 class 声明，都绝不能修改自身的 props，自身的 props 都是来源于父亲
2. state 是可变的(setState())，组件内部的值要发生变化，可以把这个值初始化为 state，通过 this.setState()改变值
3. 组件内部可以使用 state，组件与组件之间只能使用 props

### React 类组件

```js
import React from 'react';
import styles from './index.less';
import ListComponent from '@/components/List/index.jsx';

class Home extends React.Component {
  static staticNum = 11; // 子的静态变量
  constructor(props) {
    // 子的构造函数
    super(props); // 继承父类的原型对象上的方法和变量
    this.state = {
      // 子原型对象的变量
      num: 10,
    };
  }
  componentDidMount() {
    // 子的原型方法
    console.log(this);
  }
  static getDerivedStateFromProps(props, state) {
    // 子的静态方法
    console.log(props, state);
    return true;
  }
  render() {
    return (
      <div className={styles.home}>
        子类原型对象变量:{this.state.num}
        子类的静态变量:{this.constructor.staticNum}
        子类的静态变量:{this.__proto__.constructor.staticNum}
        <div className={styles.content}>
          <ListComponent />
        </div>
      </div>
    );
  }
}

export default Home;
```

### 函数式组件

- class 组件可以转换成函数式组件

```js
import React from 'react';

const Com = (props) => (
  <div>
    <button
      onClick={() => {
        props.getCountFn(props.prolist.length);
      }}
    >
      计数器
    </button>
    <ul>
      {props.prolist.map((item) => {
        return <li key={item.id}>{item.title}</li>;
      })}
    </ul>
  </div>
);
export default Com;
```

### 受控组件

- 表单元素的 value 属性，value 的值为 this.state.value
- 在 React 中，可变状态(mutable state) 通常保存在 state 属性中，并且只能通过使用 setState()来更新，使得 React 的 state 成为唯一的数据源。
- **imput 的 value 的属性的值就是输入框的值，在构造器中设定初始值 value,把 this.state.value 赋值给 input 的 value 属性**

- 渲染表单的 React 组件还控制着用户输入过程中表单发生的操作，被 React 以这种方式控制取得的表单输入元素就叫做 '受控组件'。

### 非受控组件

```html
<input type="file" />
```

- 没有 value 的值，无法控制其值
- 受控不受控，取决于表单有没有有 value 属性的值，没有非受控组件，有了并且值为初始化的为受控组件。
