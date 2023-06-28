---
title: JS循环
date: 2019-3-28
tags: JS
author: 映雪
---

绘染绚路，梨映花明，衣非冷矣。 路绘所绮，明为梨现，非失衣寒。

<!--more-->

### 循环

- 多次运行相同的代码块，获得不同的的值。

- 循环要素:

1. 停止条件
2. 声明计数器
3. 改变计数器

### for 循环

- 循环代码块一定的次数。

```js
for (var i = 0; i < 5; i++) {
  console.log('该数字为 ' + i);
}
```

### for/in 循环

- for/in 语句循环遍历对象的属性。

```js
var person = { name: '小明', age: '女', age: 56 };

for (key in person) {
  console.log('对象key' + key + ',对象value' + person[key]);
}
```

### for/of 循环

- ES6 引入，遍历类数组结构，数据结构需要有 Symbol.iterator 属性。

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

// {value:{name: '小华'},done:false}
// {value:{2: 3},done:false}
// {value: undefined, done: true}
```

### while 循环

- 只要指定条件为 true，循环就可以一直执行代码块。

```js
var i = 0;
while (i < 100) {
  console.log('该数字为 ' + i);
  i++;
}
```

### do/while 循环

- do/while 循环是 while 循环的变体。该循环会在检查条件是否为真之前执行一次代码块，然后如果条件为真的话，就会重复这个循环。

```js
var i = 1;
var sum = 1;
do {
  sum = sum * i;
  i++;
} while (i < 5);
```

- do/while 和 while 的区别，do-while 永远都比 while 多执行一次
- do/while 真正循环执行语句，在 do 后面

### 循环中的 break 和 continue

- break: 用来跳出当前循环
- continue: 跳过本次循环，直接执行下一次
