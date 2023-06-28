---
title: async
date: 2018-11-25
tags: ES6
author: 映雪
---

你遇见一个人，犯了一个错，你想弥补想还清，到最后才发现你根本无力回天。

<!--more-->

### async

- async 是 Generator 函数的语法糖。

```js
const fs = require('fs');

const readFile = function (fileName) {
  return new Promise(function (resolve, reject) {
    fs.readFile(fileName, function (error, data) {
      if (error) return reject(error);
      resolve(data);
    });
  });
};

const gen = function* () {
  const f1 = yield readFile('/etc/fstab');
  const f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

- 可以改为

```js
const asyncReadFile = async function () {
  const f1 = await readFile('/etc/fstab');
  const f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

- 优化

1. Generator 函数的执行必须靠执行器，需要调用 next 方法，或者 co 模块，而 async 函数自带执行器。也就是说，async 函数的执行，与普通函数一模一样，只要一行。
2. 返回值是 Promise 对象，而 Generator 返回的是 Interator 对象，async 可以使用 then 方法指定下一步操作。

### async 用法

- async 函数返回一个 Promise 对象，可以使用 then 方法添加回调函数。当函数执行的时候，一旦遇到 await 就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

```js
function getTime() {
  let time = 0;
  new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(1000);
    }, 1000);
  }).then((resolve) => {
    time = resolve;
  });
  console.log(time);
}
getTime();
```

- 由于 异步赋值 所以 console.log(time) 为 0，这个时候就可以使用 async

```js
async function getTime() {
  let time = 0;
  await new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(1000);
    }, 1000);
  }).then((resolve) => {
    time = resolve;
  });
  console.log(time);
}
getTime();
```

- 此时的 time 为 1000

### async 语法

- async 函数返回一个 Promise 对象。

- async 函数内部 return 语句返回的值，会成为 then 方法回调函数的参数。

```js
async function helloAsync() {
  return 'helloAsync';
}

console.log(helloAsync()); // Promise {<resolved>: "helloAsync"}

helloAsync().then((v) => {
  console.log(v); // helloAsync
});
```

- async 函数中可能会有 await 表达式，async 函数执行时，如果遇到 await 就会先暂停执行 ，等到触发的异步操作完成后，恢复 async 函数的执行并返回解析值。

- await 关键字仅在 async function 中有效。如果在 async function 函数体外使用 await ，你只会得到一个语法错误。

```js
function testAwait() {
  return new Promise((resolve) => {
    setTimeout(function () {
      console.log('testAwait');
      resolve();
    }, 1000);
  });
}

async function helloAsync() {
  await testAwait();
  console.log('helloAsync');
}
helloAsync();
// testAwait
// helloAsync
```

### await

- await 操作符用于等待一个 Promise 对象，它只能在异步函数 async function 内部使用。
- 返回 Promise 对象的处理结果。如果等待的不是 Promise 对象，则返回该值本身。
- 如果一个 Promise 被传递给一个 await 操作符，await 将等待 Promise 正常处理完成并返回其处理结果。

```js
function testAwait(x) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}

async function helloAsync() {
  var x = await testAwait('hello world');
  console.log(x);
}
helloAsync();
// hello world
```

- 正常情况下，await 命令后面是一个 Promise 对象，它也可以跟其他值，如字符串，布尔值，数值以及普通函数。

```js
function testAwait() {
  console.log('testAwait');
}
async function helloAsync() {
  await testAwait();
  console.log('helloAsync');
}
helloAsync();
// testAwait
// helloAsync
```

- await 针对所跟不同表达式的处理方式：

- Promise 对象：await 会暂停执行，等待 Promise 对象 resolve，然后恢复 async 函数的执行并返回解析值。
- 非 Promise 对象：直接返回对应的值。
