---
title: 变量提升
date: 2019-12-25
tags: JS
author: 映雪
---

一朝春尽红颜老，花落人亡两不知。

<!--more-->

### 变量提升

- 变量提升是var的一种特性

- 原理：JS引擎的工作方式是
 1. 先解析代码，获取所有被声明的变量；
 2. 然后在运行。也就是专业来说是分为预处理和执行两个阶段。

- 变量提升的定义：所有变量的声明语句都会被提升到代码头部，这就是变量提升.


### 例子

```js
console.log(a);
var a =1;
```

- 不会报错，只会提示undefined,实际在js引擎中的运行过程是：

```js
var a;
console.log(a);
a =1;
```

- 实际运行表示变量a已声明，但还未赋值。

- **但是变量提升只对var命令声明的变量有效，如果一个变量不是用var命令声明的，就不会发生变量提升。**

```js
console.log(aa);
let aa =1;
```

- 报错：ReferenceError: aa is not defined


### function 提升


```js
a();

function a(){
    console.log(1);
};
```

- 表面上，上面代码好像在声明之前就调用了函数a。但是实际在js引擎中，由于“变量提升”，函数a定义部分被提升到了代码头部，也就是在调用之前已经声明了。
但是,如果采用赋值语句定义函数，JavaScript就会报错：

```js
a();

var a = function(){
    console.log(1);
};
```

- 因为js引擎把变量声明提升，此时，a就是一个变量，而并不是一个function，以下是js引擎实际运行代码：

```js
var a;
a();

a = function(){
    console.log(1);
};

```