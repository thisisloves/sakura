---
title: this
date: 2018-5-20
tags: JS
author: 映雪
---

浮云吹作雪，世味煮成茶。

<!--more-->

### this 关键字

- 面向对象语言中 this 表示当前对象的一个引用。
- 但在 JavaScript 中 this 不是固定不变的，它会随着执行环境的改变而改变。

### this 使用

1.  对象方法中的 this

- 在方法中，this 表示该方法所属的对象。

```js
var obj = {
  name: '小明',
  age: '23',
  info: function () {
    console.log(this); // obj对象
    return this.name + this.age;
  },
};
obj.info();
```

2. 单独使用的 this

- 如果单独使用，this 表示全局对象。

```js
console.log(this); // Window.Object
```

3. 函数中使用的 this

- 在函数中，this 表示全局对象。

```js
function fn() {
  this.a = 1;
  return this;
}

console.log(fn()); // Window.Object
```

4. 作为构造函数调用

- new 关键字会创建一个空的对象，然后会自动调用一个函数 apply 方法，将 this 指向这个空对象，这样的话函数内部的 this 就会被这个空的对象替代。

```js
function f() {
  this.a = 1;
  console.log(this); // f()
  return this;
}
const b = new f();
console.log(b.a); // 1
```

5. 在函数中，在严格模式下使用 this

- this 是未定义的(undefined)

```js
'use strict';
function myFunction() {
  return this;
}
```

6. 在事件中使用 this

- this 表示接收事件的元素。

```html
<button onclick="this.style.display='none'">点我后我就消失了</button>
```

### this 理解

- this 的对象是运行时基于函数的执行环境绑定的，它的指向完全取决于函数的调用方式

```js
var name = 'The Window';
var object = {
  name: 'My Object',
  getNameFunc: function () {
    return function () {
      return this.name;
    };
  },
};
alert(object.getNameFunc()()); //"The Window"（在非严格模式下）
```

```js
var name = 'The Window';
var object = {
  name: 'My Object',
  getNameFunc: function () {
    var that = this;
    return function () {
      return that.name;
    };
  },
};
alert(object.getNameFunc()()); //"My Object"
```

- 理解

1. 第 1 个例子里是在全局环境中调用了这个函数，所以 this 关键字最终指向了全局对象 The Window，从而利用闭包在全局环境中调用了 The Window；
2. 第 2 个例子不同，它返回的是 that 的 name，而 this 对象被赋值给了 that 变量，就是说 this 对象和 that 捆绑在了一起，那么在调用这个方法时相当于在 object 里利用闭包去寻找最终变量（that 是引用着 object 的），所以只能找到 My Object 变量。

### this 重定义

- call()、apply()、bind() 都是用来重定义 this 这个对象的

```js
var people = { name: '小王', age: '23' };
var other = {
  name: '小明',
  age: '22',
  function: function () {
    console.log('我的名字叫' + this.name + '我的年龄为' + this.age);
  },
};
other.function(); // 我的名字叫小明我的年龄为22
other.function.call(people); // 我的名字叫小王我的年龄为23
other.function.apply(people); // 我的名字叫小王我的年龄为23
other.function.bind(people)(); // 我的名字叫小王我的年龄为23
```

- 传参

```js
var people = { name: '小王', age: '23' };
var other = {
  name: '小明',
  age: '22',
  function: function (from, go) {
    console.log(
      '我的名字叫' + this.name + '我的年龄为' + this.age + '，从' + from + '来，去往' + go,
    );
  },
};
other.function('成都', '上海'); // 我的名字叫小明我的年龄为22，从成都来，去往上海
other.function.call(people, '成都', '上海'); // 我的名字叫小王我的年龄为23，从成都来，去往上海
other.function.apply(people, ['成都', '上海']); // 我的名字叫小王我的年龄为23，从成都来，去往上海
other.function.bind(people, ['成都', '上海'])(); // 我的名字叫小王我的年龄为23，从成都,上海来，去往undefined
```
