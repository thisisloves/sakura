---
title: React组件通信
date: 2019-3-6
tags: React
author: 映雪
---

未曾相逢先一笑，初会便已许平生。

<!--more-->

### 组件通信

- 组件实例的作用域是相互独立的，这就意味着不同组件之间的数据无法相互引用。因此组件与组件之间需要有通信。
- 子组件的关系可以总结为 props 向下传递，事件向上传递。父组件通过 props 给子组件下发数据，子组件通过事件给父组件发送信息。

### 父组件给子组件传值

- Home 为父组件，ProList 为子组件
- 一般子组件负责数据的展示，父组件负责传递数据给子组件

- Home.js 组件中 数据请求拿到数据

```js
class Com extends Component {
  constructor(props) {
    super(props);
    this.state = {
      prolist: [],
    };
  }
  componentDidMount() {
    fetch('http://www.daxunxun.com/douban').then((res) => {
      this.setState({
        prolist: data,
      });
    });
  }
}
```

- Home 传值给 ProList
- 父组件调用子组件的地方，添加了一个自定义的属性，属性的值就是想要传递给子组件的值

- Home.js

```js
render () {
  return (
    <div className="box">
    <header className="header">首页头部</header>
    <div className="content">首页内容
    <Prolist prolist={this.state.prolist}>
    </div>
    </div>
  )
}
```

- 子组件 Prolist 接受数据

```js
import React, { Component } from 'react';

class Com extends Component {
  render() {
    console.log(this);
    return (
      <ul>
        <li>肖申克的救赎</li>
      </ul>
    );
  }
}
export default Com;
```

- 子组件 Prolist 渲染数据

```js
import React, {Component } from 'react';

calss Com extends Component  {
  render () {
    console.log(this)
    return (
      <ul>
      {
        this.props.prolist.map(item=>{
          return (<li key ={item.id}>{item.title}</li>)
        })
      }
      </ul>
    )
  }
}
```

### 子组件给父组件传值

- 父组件在调用子组件的地方，添加了一个自定义的属性，属性的值为一个函数，函数内部接受未来子组件
  传递的数据，父组件可以依据数据做出相应的操作

```js
getCountFn(data){
  this.setState({
    count:data
  })
}
render () {
  return (
    <div className="box">
    <header className="header">首页头部</header>
    <div className="content">
    首页内容---{this.state.count}
    <ProList prolist ={this.state.prolist} getCountFn={this.getCountFn.bind(this)}/>
    </div>
    </div>
  )
}
```

- 子组件中通过 this.props 可以拿到父组件传递过来的所有东西，子组件中通过事件或者钩子函数给父组件传值
  调用 this.props 自定义的属性名(传递的参数)

- ProList

```js
class Com extends Component {
  render() {
    console.log(this);
    return (
      <div>
        <button
          onClick={() => {
            this.props.getCountFn(this.props.prolist.length);
          }}
        >
          计数
        </button>
      </div>
    );
  }
}
```
