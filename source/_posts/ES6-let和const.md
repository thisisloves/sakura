---
title: let和const
date: 2018-8-15
tags: ES6
author: 映雪
---

我跨过山，涉过水，见过万物复苏，周而复始，如今山是你，水也是你。

<!--more-->

### let 变量

- 用法类似于 var，但是所声明的变量，只在 let 命令所在的代码块内有效。

```js
{
  let a = 10;
  var b = 1;
}

a; // ReferenceError: a is not defined.
b; // 1
```

### let 特点

1. 不存在变量提升

- var 命令会发生“变量提升”现象，即变量可以在声明之前使用，值为 undefined。这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用。
- 为了纠正这种现象，let 命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。

```js
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```

2. 暂时性死区

- 只要块级作用域内存在 let 命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。

```js
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```

3. 不允许重复声明

- let 不允许在相同作用域内，重复声明同一个变量。

```js
// 报错
function func() {
  let a = 10;
  var a = 1;
}

// 报错
function func() {
  let a = 10;
  let a = 1;
}
```

### const 常量

- 是一个只读的常量，一旦声明，不可修改。

### const 使用

```js
const a = 10;
a = 20;
console.log(a);
```

- 结果:‘Assignment to constant variable’

```js
const arr = [1, 2];
arr = [3, 4];
console.log(arr);
```

- 结果:‘Assignment to constant variable’

```js
const obj = { key: 1 };
obj = { key: 2 };
console.log(obj);
```

- 结果:‘Assignment to constant variable’

```js
const arr = [1, 2];
arr[2] = 3;
console.log(arr);
```

- 结果:[1,3]

```js
const obj = { key: 1 };
obj.key = 2;
console.log(obj);
```

- 结果: {key: 2}

```js
let arr = [1, 2];
arr = [3, 4];
console.log(arr);
```

- 结果:[3,4]

```js
let obj = { key: 1 };
obj = { key: 2 };
console.log(obj);
```

- 结果:{key:2}

### const 本质

- const 实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。
- '基本数据类型'的值就保存在内存地址中，所以 const 定义的 ‘基础数据类型’ 不可被改变。
- '引用数据类型’ 指向的内存地址只是一个指针，通过指针来指向实际数据，不可被改变的是指针，而不是数据，所以 const 定义的 '引用数据类型的' 常量可以通过属性来修改值。

![截屏2021-12-08 下午3.59.40.png](/images/2021/12/08/bnYRF2DHopqjgCh.png)

- 基本数据类型的变量和值都在 '栈内存' 中，指向的内存地址不可被修改
- 引用数据类型的 变量存储在 '栈内存' 中，值存储在 '堆内存' 中，通过指针来指向 '堆内存' 中对应的 值，所以，const 定义的 引用数据类型，不可被改变的是 '指针' ，所以可以通过 属性来修改值。
