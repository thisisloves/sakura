---
title: Event Loop
date: 2020-8-9
tags: JS
author: 映雪
---

他会再无忧虑也再无恐惧，四季转换，岁月静好。许多年后他们要是有机会还能相逢一笑，这可能是他们最好的结果。

<!--more-->

### Event Loop(事件轮循)

- js 执行代码的过程是顺序执行，也就是从上往下执行，也就是说，当执行代码时，第一个任务先执行完，才会执行下一个任务，但是如果中途有任务执行耗时太长，又不能让后面任务等待，这个时候就需要分成俩个任务了，同步任务和异步任务,等同步任务也就是主线程任务执行完成，会到异步队列里面拿取任务。

1. js 是单线程语言
2. JavaScript 执行机制是 Event Loop

![截屏2023-06-21 下午3.05.27.png](/images/2023/06/21/ZqnuVzfcCdvxYDE.png)

### 异步任务分为宏任务和微任务

1. 首先我们进入异步队列中会先检测微任务中是否有可执行的代码
2. 如果有任务存在，则执行完所以的微任务
3. 没有微任务存在，则重新执行宏任务，往此反复

### 同步任务，异步任务图解

![截屏2021-07-05 下午5.13.57.png](/images/2021/07/05/s7rEQ5P4ih1weFY.png)

### 宏任务微任务执行图解

![截屏2021-07-05 下午5.13.38.png](/images/2021/07/05/6doQ9kDNcRMvTat.png)

**1.同步任务先执行，但是这里同步任务其实也是宏任务**
**2.异步队列是先判断是否有可执行的微任务**

```js
console.log('start');

setTimeout(() => {
  console.log('setTimeout');

  setTimeout(() => {
    console.log('2222');
  }, 0);

  Promise.resolve()
    .then(function () {
      console.log('promise3');
    })
    .then(function () {
      console.log('promise4');
    });

  new Promise((resolve, reject) => {
    console.log(1111);
  });
}, 0);

Promise.resolve()
  .then(function () {
    console.log('promise1');
  })
  .then(function () {
    console.log('promise2');
  });

console.log('end');
```

```
//答案如下
start
end
promise1
promise2
setTimeout
1111
promise3
promise4
2222
```

- 执行过程：

1. 先执行同步任务，所以先输出 start，end，因为 console.log 是同步任务，其实也是宏任务，直接执行。
2. 然后执行 Promise.then 方法，是异步队列里面的微任务，输出 promise1，promise2.
3. 最后执行存入异步队列里面的 setTimeout 方法。但是在这里面也分为宏任务，微任务，先执行 console，输出 setTimeout。
4. 这里特别注意，new 实例化的 promise 和 Promise 之间不一样，new promise 是宏任务，先执行，在执行微任务 Promise，最后执行 setTimeout。
