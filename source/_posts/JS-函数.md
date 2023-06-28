---
title: JS函数
date: 2019-3-21
tags: JS
author: 映雪
---

墙隅篱下黄蕊开， 道是秋菊去年栽。 满园金色拾不尽， 嫩芯繁瓣清风裁。

<!--more-->

### 函数

- 函数是由事件驱动的或者当它被调用时执行的可重复使用的代码块。

### 函数创建

1. 赋值式

```js
var a = function () {};
```

2. 声明式

```js
function a() {}
```

### 函数的分类

1. 正常函数

```js
function fn() {} //正常函数
```

2. 匿名函数

```js
const fn = function () {};
```

3. 即时函数

- 即时函数是匿名函数的另一种应用，即时函数就是函数在定义后可以被立即调用。

```js
(function () {})(); //自动立即执行
```

### 函数的参数

- 参数类型

1. 形参

- 定义函数名和函数体的时候使用的参数，目的是用来接收调用该函数时传递的参数。

2. 实参

- 实参是用来填充形参的。当函数被调用时，实参列在函数名后面的括号里。执行函数调用时，实参被传递给形参。

```js
function fn(a,b,c,d)//形参{
   console.log(a);
   console.log(b);
   console.log(c);
   console.log(d);
}
fn(10,20,30,40) //实参
```

- 实参和形参的数量一致，一一对应
- 实参比形参少，多出来的形参是 undefined
- 实参比形参多，多出来的实参，可以找 arguments

### 函数的返回(return)

- 让函数有返回值的关键字，终止函数，每个函数都有返回值，没有 return 的函数，返回的是 undefined，有 return 的，返回的 return 的值。

```js
const fn = function () {};
const fn1 = function () {
  return '返回值';
};
console.log(fn()); // undefied
console.log(fn1()); // 返回值
```

### arguments

- 不确定实参传递个数的时候，获取参数

```js
function fn() {
  console.log(arguments); // Arguments(4) [1, 4, 5, 6, callee: ƒ, Symbol(Symbol.iterator): ƒ]
  var i;
  var num = 0;
  for (i = 0; i < arguments.length; i++) {
    num = arguments[i] + num;
  }
  console.log(num);
}
fn(1, 4, 5, 6);
```
