---
title: Promise对象
date: 2018-12-20
tags: ES6
author: 映雪
---

或许前路永夜，即便如此我也要前进，因为星光即使微弱，也会为我照亮前路。

<!--more-->

### 背景

- 实际开发中，想让程序按顺序执行，因为函数几乎同步执行，各自执行各自的，互相之间不会等待。 有一种方式可以解决，回调函数。但是执行的先后执行的任务多了，就会形成很深的嵌套结构，造成回调地域，极其不优雅，极其不便于维护。所以就有了promise。

### Promise

- Promise 是异步编程的一种解决方案。
- Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。
- Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

### Promise 特点

1. 对象的状态不受外界影响。Promise 对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和 rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是 Promise 这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise 对象的状态改变，只有两种可能：从 pending 变为 fulfilled 和从 pending 变为 rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对 Promise 对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

### Promise 缺点

- 无法取消 Promise ，一旦新建它就会立即执行，无法中途取消。
- 如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。
- 当处于 pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

### Promise 使用

```js
const promise = new Promise(function (resolve, reject) {
  let timeout = 0;
  setTimeout(() => {
    timeout = (Math.random() * 20).toFixed(0);
    if (timeout > 10) {
      resolve(timeout);
    } else {
      reject(new Error(timeout));
    }
  }, 1000);
});

promise.then(
  (resolve) => {
    console.log(resolve);
  },
  (reject) => {
    console.log(reject);
  },
);
```

### Promise.prototype.then()

- then 方法是定义在原型对象 Promise.prototype 上的。它的作用是为 Promise 实例添加状态改变时的回调函数。
- then 方法接收两个函数作为参数，第一个参数是 Promise 执行成功时的回调，第二个参数是 Promise 执行失败时的回调，两个函数只会有一个被调用。

```js
promise
  .then(
    (resolve) => {
      console.log(resolve);
      return resolve*2;
    },
    (reject) => {
      console.log(reject);
      return reject;
    },
  )
  .then(
    (resolve) => {
      console.log(resolve);
    },
    (reject) => {
      console.log(reject);
    },
  );
```

- 注意

1. 简便的 Promise 链式编程最好保持扁平化，不要嵌套 Promise。
2. 大多数浏览器中不能终止的 Promise 链里的 rejection，建议后面都跟上 .catch(error => console.log(error));

### Promise.prototype.catch()

- Promise.prototype.catch()方法是.then(null, rejection)或.then(undefined, rejection)的别名，用于指定发生错误时的回调函数。

```js
  promise.then(resolve=>{
      console.log(resolve)
  }).catch(error=>{
      console.log(error)
  })
```

- 等同于

```js
promise.then(
  (resolve) => {
    console.log(resolve);
  },
  (reject) => {
    console.log(reject);
  },
);
```

### Promise.prototype.finally()

- finally()方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。

```js
  promise.then(resolve=>{
      console.log(resolve)
  }).catch(error=>{
      console.log(error)
  }).finally(()=>{
      console.log('这是 finally !')
  })
```

### Promise.all()

- Promise.all()方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

- Promise.all()方法接受一个数组作为参数，p1、p2、p3都是 Promise 实例，如果数组成员不是 Promise 实例，就会先调用Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。
- Promise.all()方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。

```js
    const p1 = new Promise(function (resolve, reject) {
        let timeout = 0
        setTimeout(() => {
            timeout = (Math.random() * 20).toFixed(0)
            if (timeout > 10) {
                resolve(timeout)
            } else {
                console.log('p1抛出错误')
                reject(new Error(timeout))
            }
        }, 3000)

    })

    const p2 = new Promise(function (resolve, reject) {
        let timeout = 0
        setTimeout(() => {
            timeout = (Math.random() * 20).toFixed(0)
            if (timeout > 10) {
                resolve(timeout)
            } else {
                console.log('p2抛出错误')
                reject(new Error(timeout))
            }
        }, 1000)

    })

    const  p3 = 1;

    const p =  Promise.all([p1,p2,p3])

    p.then((resolve)=>{
        console.log(resolve)
    }).catch((error)=>{
        console.log(error)
    })
```

- 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
- 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

- 注意
1. Promise.all获得的成功结果的数组里面的数据顺序和Promise.all接收到的数组顺序是一致的，即p1的结果在前，即便p1的结果获取的比p2要晚。
2. 发送多个请求并根据请求顺序获取和使用数据的场景，使用Promise.all最合适不过。



### Promise.race()

- Promise.race()方法是将多个 Promise 实例，包装成一个新的 Promise 实例。

```js
const p = Promise.race([p1, p2, p3]);

p.then((resolve)=>{
        console.log(resolve)
    }).catch((error)=>{
        console.log(error)
    })
```

- p里面哪个结果获得的快，就返回那个结果，不管结果本身是成功状态还是失败状态。