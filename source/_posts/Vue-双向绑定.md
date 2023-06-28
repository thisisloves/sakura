---
title: Vue双向绑定
date: 2019-5-20
tags: Vue
author: 映雪
---

看尽人间繁华，三千浮生若水。

<!--more-->

### Vue 数据双向绑定

- 数据变化更新视图，视图变化更新数据。

![截屏2022-01-13 下午4.40.56.png](/images/2022/01/13/DJ1W7mRwPQbfT3M.png)

- 视图文本节点内容变化时，Data 中的数据同步变化。即 View => Data 的变化。
- Data 中的数据变化时，文本节点的内容同步变化。即 Data => View 的变化。

- 其中，View 变化更新 Data ，可以通过事件监听的方式来实现，所以 Vue 的数据双向绑定的工作主要是如何根据 Data 变化更新 View。

### 数据双向绑定实现

- 数据劫持的实现方式 **Object.defineProperty()**
- 当你把一个普通的 JavaScript 对象传入 Vue 实例作为 data 选项，Vue 将遍历此对象所有的属性，并使用 Object.defineProperty 把这些属性全部转为 getter/setter。

```js
let Book = { name: '' };
let tem = Book.name;
Object.defineProperty(Book, 'name', {
  get: function () {
    return tem;
  },
  set: function (val) {
    console.log('我修改了对象');
    tem = val;
  },
});
console.log(Book.name);
Book.name = '平凡的世界';
console.log(Book.name);
```

1. 实现一个监听器 Observer：对数据对象进行遍历，包括子属性对象的属性，利用 Object.defineProperty() 对属性都加上 setter 和 getter。这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化。

2. 实现一个解析器 Compile：解析 Vue 模板指令，将模板中的变量都替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，调用更新函数进行数据更新。

3. 实现一个订阅者 Watcher：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁 ，主要的任务是订阅 Observer 中的属性值变化的消息，当收到属性值变化的消息时，触发解析器 Compile 中对应的更新函数。

4. 实现一个订阅器 Dep：订阅器采用 发布-订阅 设计模式，用来收集订阅者 Watcher，对监听器 Observer 和 订阅者 Watcher 进行统一管理。


![938664-20170522225458132-1434604303 [2].png](/images/2019/05/20/5ce29d426dd1270796.png)
