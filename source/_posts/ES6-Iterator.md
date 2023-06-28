---
title: Iterator
date: 2020-10-8
tags: ES6
author: 映雪
---

清影渺难即，飞絮满天涯。

<!--more-->

### Iterator

- 是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

### Iterator 遍历

1. 创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。
2. 第一次调用指针对象的 next 方法，可以将指针指向数据结构的第一个成员。
3. 第二次调用指针对象的 next 方法，指针就指向数据结构的第二个成员。
4. 不断调用指针对象的 next 方法，直到它指向数据结构的结束位置。

```js
function makeIterator(array) {
  let nextIndex = 0;
  return {
    next: function () {
      return nextIndex < array.length
        ? { value: array[nextIndex++], done: false }
        : { value: undefined, done: true };
    },
  };
}
const iterator = makeIterator([1.3, 4]);

console.log(iterator.next()); // {value: 1.3, done: false}
console.log(iterator.next()); //  {value: 4, done: false}
console.log(iterator.next()); //  {value: undefined, done: true}
```

### Iterator 接口

- 默认的 Iterator 接口部署在数据结构的 Symbol.iterator 属性，或者说，一个数据结构只要具有 Symbol.iterator 属性，就可以认为是“可遍历的”（iterable）。Symbol.iterator 属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器。至于属性名 Symbol.iterator，它是一个表达式，返回 Symbol 对象的 iterator 属性，这是一个预定义好的、类型为 Symbol 的特殊值，所以要放在方括号内。

- 原生具备 Iterator 接口的数据结构

1. Array
2. Map
3. Set
4. String
5. TypedArray
6. 函数的 arguments 对象
7. NodeList 对象

```js
const arr = [3, 4, '22'];
const iter = arr[Symbol.iterator]();

console.log(iter.next()); // {value:3,done:false}
console.log(iter.next()); // {value:4,done:false}
console.log(iter.next()); // {value:'22',done:false}
console.log(iter.next()); // {value:undefined,done:true}
```

- 注意:**对象无 Iterator 接口，因为对象是没有顺序的，不是线性结构。给一个对象部署 Interator 接口就是对对象线性转化。**

### 对象部署 Interator 接口

```js
let obj = {
  1: 444,
  2: 44,
};
obj[Symbol.iterator] = function () {
  let keys = Object.keys(this);
  let index = 0;
  return {
    next: () => {
      if (index < keys.length) {
        //>= keys数组长度, 遍历结束, 返回如下对象(为了告诉for...of遍历结束)
        let key = keys[index];
        index++;
        return {
          value: { [key]: this[key] },
          done: false,
        };
      } else {
        return {
          value: undefined,
          done: true,
        };
      }
    },
  };
};

for (let item of obj) {
  console.log(item);
}
```

### 重写数组 Interator 接口

```js
const arr = [3, 4, '22'];
arr[Symbol.iterator] = function makeIterator() {
  let that = this;
  let nextIndex = 0;
  console.log(this);
  return {
    next: function () {
      return nextIndex < that.length
        ? { value: '重写的:' + that[nextIndex++], done: false }
        : { value: undefined, done: true };
    },
  };
};
const ite = arr[Symbol.iterator]();
console.log(ite.next());
console.log(ite.next());
console.log(ite.next());
console.log(ite.next());
for (let item of arr) {
  console.log(item);
}
```

![截屏2022-01-18 下午4.19.23.png](/images/2022/01/18/OVDiyBUo1qprHmK.png)
