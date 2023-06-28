---
title: Generator
date: 2020-9-15
tags: ES6
author: 映雪
---

沙罗双树之花色，表盛者必衰之兆， 骄者难久，恰如春宵一梦； 猛者遂灭，好似风前之尘。

<!--more-->

### Generator

- Generator 函数是 ES6 提供的一种异步编程解决方案。
- Generator 函数是一个状态机，封装了多个内部状态。
- Generator 函数执行会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。

- 与普通区别

1. Generator 和函数不同的是，Generator 由 function* 定义（注意多出的*号）。
2. 函数体内部使用 yield 表达式，定义不同的内部状态。
3. 除了 return 语句，还可以用 yield 返回多次。

```js
function* foo(x) {
  yield x + 1;
  yield x + 2;
  return x + 3;
}
console.log(foo(1).next()); // {value: 2, done: false}
console.log(foo(1).next()); // {value: 2, done: false}
console.log(foo(1).next()); // {value: 2, done: false}
console.log(foo(1).next()); // {value: 2, done: false}

let ad = foo(1);
console.log(ad.next()); // {value: 2, done: false}
console.log(ad.next()); // {value: 3, done: false}
console.log(ad.next()); // {value: 4, done: false}
console.log(ad.next()); // {value: undefined, done: true}
```

### yield 表达式

* Generator 函数返回的遍历器对象，只有调用 next 方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。yield 表达式就是暂停标志。

- 运行逻辑

1. 遇到 yield 表达式，就暂停执行后面的操作，并将紧跟在 yield 后面的那个表达式的值，作为返回的对象的 value 属性值。
2. 下一次调用 next 方法时，再继续往下执行，直到遇到下一个 yield 表达式。
3. 如果没有再遇到新的 yield 表达式，就一直运行到函数结束，直到 return 语句为止，并将 return 语句后面的表达式的值，作为返回的对象的 value 属性值。
4. 如果该函数没有 return 语句，则返回的对象的 value 属性值为 undefined。

- yield 与 return 区别

1. 都能返回紧跟在语句后面的那个表达式的值。
2. 每次遇到 yield，函数暂停执行，下一次再从该位置继续向后执行，而 return 语句不具备位置记忆的功能。
3. 一个函数里面，只能执行一次（或者说一个）return 语句，但是可以执行多次（或者说多个）yield 表达式。
4. 正常函数只能返回一个值，因为只能执行一次 return；Generator 函数可以返回一系列的值，因为可以有任意多个 yield。

### Generator 部署 Interator 接口

```js
let obj = { name: '小华', 2: '3' };
obj[Symbol.iterator] = function* () {
  let keys = Object.keys(this);
  for (let i = 0; i < keys.length; i++) {
    yield { value: { [keys[i]]: this[keys[i]] }, done: false };
  }
  yield { value: undefined, done: true };
};
for (let item of obj) {
  console.log(item);
}
```

### next 方法

- next 方法可以带一个参数，该参数就会被当作**上一个 yield 表达式的返回值**。
- 可以在 Generator 函数运行的不同阶段，从外部向内部注入不同的值，从而调整函数行为。

```js
function* foo(e) {
  let x = yield e;
  let y = yield e + x;
  yield x + y;
}
let add = foo(1);
// console.log(add.next())   // {value:1,done:false}
// console.log(add.next())   // {value: NaN, done: false}
// console.log(add.next())   // {value: NaN, done: false}

console.log(add.next()); // {value:1,done:false}
console.log(add.next(1)); // {value:2,done:false}
console.log(add.next(2)); // {value:3,done:false}
```

- next 不传参数情况下，第一次 next() value 为 1 ， 第二次 next() value 等于 1 加上次 yield e 的值 即 undefined + 1 ，第三次 next() 为 value 等于 undefined 加 undefined 的值 即 undefined + undefined

- next 传参数情况下，第一次 next() value 为 1 ， 第二次 next(1) value 等于 1 加上次 yield e 的值 即 1 + 1 ，第三次 next() 为 value 等于 1 加 2 的值 即 1 + 2

### Generator.prototype.throw()

- Generator 函数返回的遍历器对象，都有一个throw方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。

```js
var g = function* () {
  try {
    yield;
  } catch (e) {
    console.log('内部捕获', e);
  }
};

var i = g();
i.next();

try {
  i.throw('a');
  i.throw('b');
} catch (e) {
  console.log('外部捕获', e);
}
// 内部捕获 a
// 外部捕获 b
```

- throw方法可以接受一个参数，该参数会被catch语句接收，建议抛出Error对象的实例。

### Generator.prototype.return() 

- Generator 函数返回的遍历器对象，还有一个return()方法，可以返回给定的值，并且终结遍历 Generator 函数。

```js
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

var g = gen();

g.next()        // { value: 1, done: false }
g.return('foo') // { value: "foo", done: true }
g.next()        // { value: undefined, done: true }
```

### yeild* 

- 用来在一个 Generator 函数里面执行另一个 Generator 函数。

```js
function* bar() {
  yield 'x';
  yield* foo();
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  yield 'a';
  yield 'b';
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  for (let v of foo()) {
    yield v;
  }
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
// "x"
// "a"
// "b"
// "y"
```