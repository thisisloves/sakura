---
title: Promise手写
date: 2018-12-21
tags: JS
author: 映雪
---

安静地阅读一本书，嘴角带着静如止水的笑。

<!--more-->

### Promise

- Promise 是异步编程的一种解决方案。
- Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。
- Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

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

```

### Promise 状态

1. pending: 初始状态，既不是成功，也不是失败状态。
2. fulfilled: 意味着操作成功完成。
3. rejected: 意味着操作失败。

### Promise 实现

```js
/**
 * 创建三变量记录表示状态
 * 用that保存this，避免后期闭包导致this的指向不对
 * value 变量用于保存 resolve 或者 reject 中传入的值
 * resolvedCallbacks 和 rejectedCallbacks 用于保存 then 中的回调，
 * 因为当执行完 Promise 时状态可能还是等待中，这时候应该把 then 中的回调保存起来用于状态改变时使用
 */

const PENDING = 'pending';
const FULFILLED = 'fulfilled';
const REJECTED = 'rejected';

function myPromise(fn) {
  const that = this;
  that.value = null;
  that.status = PENDING; //默认状态
  that.fulfilledCallbacks = [];
  that.rejectedCallbacks = [];
  function resolve(value) {
    if (that.status === PENDING) {
      that.status = FULFILLED;
      that.value = value;
      //执行回调方法
      that.fulfilledCallbacks.forEach((myFn) => myFn(that.value));
    }
  }
  function reject(value) {
    if (that.status === PENDING) {
      that.status = REJECTED;
      that.value = value;
      //执行回调方法
      that.rejectedCallbacks.forEach((myFn) => myFn(that.value));
    }
  }

  // 执行回调函数
  try {
    fn(resolve, reject);
  } catch (e) {
    reject(e);
  }
}

myPromise.prototype.then = function (onFulfilled, onRejected) {
  let self = this;
  //等待状态，则添加回调函数到栈中
  if (self.status === PENDING) {
    self.fulfilledCallbacks.push(() => {
      onFulfilled(self.value);
    });
    self.rejectedCallbacks.push(() => {
      onRejected(self.value);
    });
  }
  if (self.status === FULFILLED) {
    onFulfilled(self.value);
  }

  if (self.status === REJECTED) {
    onRejected(self.value);
  }
};

let p = new myPromise((resolve, reject) => {
  console.log('hello');
  resolve(5);
});
p.then((res) => {
  console.log(res);
});
p.then(() => {
  console.log('jj');
});
```
