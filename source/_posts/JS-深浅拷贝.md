---
title: 深浅拷贝
date: 2019-3-24
tags: JS
author: 映雪
---

真正的情谊，无关风月，纤尘不染，只因你在，我也在，你未离，我亦未弃。羁绊，本就如此简单。

<!--more-->

### 浅拷贝

- 浅拷贝（shallowCopy）只是增加了一个指针指向已存在的内存地址。

### 深拷贝

- 深拷贝（deepCopy）是增加了一个指针并且申请了一个新的内存，使这个增加的指针指向这个新的内存。

### 理解

- 深拷贝和浅拷贝最根本的区别在于是否真正获取一个对象的复制实体，而不是引用。
- 假设 B 复制了 A，修改 A 的时候，看 B 是否发生变化，如果 B 跟着也变了，说明是浅拷贝，反之为深拷贝。
- 深拷贝是先开辟出一块空间，再将值拷贝进去。

### 数组拷贝操作

1. 直接赋值

```js
let a = [1, 2, [3, 4]];
let a1 = a;
a1[0] = 0;
console.log(a, a1); // [0,2,[3,4]] [0,2,[3,4]]
```

- 原数组和复制数组值同时改变，为浅拷贝

2. 直接修改原数组值的方法 pop reverse shift unshift push splice

```js
let a = [1, 2, [3, 4]];
a.push(2);
let a2 = a;
console.log(a, a2); // [1,2,[3,4],2]  [1,2,[3,4],2]

let a3 = splice(0, 1, 0);
console.log(a, a3); // [0, 2, Array(2), 2]  [1]
```

- 直接修改原数组的方法，无拷贝

3. 扩展运算符、slice、 concat

```js
let a = [1, 2, [3, 4]];
let a0 = [4];

let a4 = [0, ...a];
console.log(a, a4); // [1, 2, [3,4]]  [0, 1, 2, [3,4]]
a4[3][1] = 0;
console.log(a, a4); // [1, 2, [3,0]]  [0, 1, 2, [3,0]]

let a5 = a.concat(a0);
console.log(a, a5); // [1, 2, [3,4]]  [1, 2, [3,4] ,4]
a5[2][1] = 0;
console.log(a, a5); // [1, 2, [3,0]]  [1, 2, [3,0] ,4]

let a6 = a.slice(1, 3);
console.log(a, a6); // [1, 2, [3,4]]  [2, [3,4]]
a6[1][1] = 0;
console.log(a, a6); // [1, 2, [3,0]]  [2, [3,0]]
```

- 当原数组为一维数组的时候，使用以上方法，修改复制数组，不改变原数组，为深拷贝
- 当原数组为二维数组的时候，使用以上方法，修改复制数组数组内的数组，会改变原数组数组内的数组，为浅拷贝

4. join、flat 、String

```js
let a = [1, 2, [3, 4]];

let a7 = a.join(',');
console.log(a, a7, a7.split(',')); // [1, 2, [3,4]]   '1,2,3,4'   ['1', '2', '3', '4']

let a8 = a.flat();
a8[4] = 6;
console.log(a, a8); //  [1, 2, [3,4]]   [1, 2, 3, 4, 99]

let a8 = String(a);
console.log(a, a8); // [1, 2, [3,4]]  '1,2,3,4'
```

- 以上复制改变了数组维度或者数据类型，不会影响到原数组，但是会存在丢失原数组的问题

5. map 、forEach

```js
let a = [1, 2, [3, 4]];
let a9 = a.map((val) => val);
a9[1] = 0;
console.log(a, a9); // [1, 2, [3,4]]  [ 1 , 0 , [3,4]]
a9[2][0] = 1;
console.log(a, a9); // [1, 2, [1,4]]  [ 1 , 0 , [1,4]]
let a10 = [];
a.forEach((val) => {
  a10.push(val);
});
a10[1] = 0;
console.log(a, a10); // [1, 2, [3,4]]  [ 1 , 0 , [3,4]]
a10[2][0] = 1;
console.log(a, a10); // [1, 2, [1,4]]  [ 1 , 0 , [1,4]]
```

- 和扩展运算符一样只能第一层数组改变不会改变原数组

6. JSON 转化

```js
let a = [1, 2, [3, 4], { name: '小华' }, () => console.log('1')];

let a11 = JSON.parse(JSON.stringify(a));
a11[0] = 0;
console.log(a, a11); // [1, 2,[3,4], {name:'小华'}, ƒ]   [0, 2,[3,4], {name:'小华'}, null ]
a11[2][0] = 1;
console.log(a, a11); // [1, 2,[3,4], {name:'小华'}, ƒ]   [0, 2,[1,4], {name:'小华'}, null ]
```

- 经过 json 转化的复制数组，修改多层不会影响到原数组，但是转化过程会存在丢失原数组的问题

7. 递归

```js
let a = [1, 2, [3, 4], { name: '小华' }, () => console.log('1')];

const deepClone = (arr) => {
  return arr.map((value) => {
    if (Array.isArray(value)) {
      return deepClone(value);
    } else {
      return value;
    }
  });
};

const a12 = deepClone(a);

a12[0] = 0;
console.log(a, a12); // [1, 2,[3,4], {name:'小华'}, ƒ]   [0, 2,[3,4], {name:'小华'}, ƒ ]
a12[2][0] = 1;
console.log(a, a12); // [1, 2,[3,4], {name:'小华'}, ƒ]   [0, 2,[1,4], {name:'小华'}, ƒ ]
```

- 递归遍历复制数组，修改多层不会影响到原数组，也不会丢失数组项

### 对象拷贝操作

1. 直接赋值

```js
let obj = { name: '小明', function: () => console.log('1'), age: 23, say: { like: '运动' } };

let obj1 = obj;
obj1.name = '小华';
console.log(obj, obj1);
```

- 原对象和复制对象值同时改变，为浅拷贝

2. Object.assign

```js
let obj = { name: '小明', function: () => console.log('1'), age: 23, say: { like: '运动' } };

let obj2 = Object.assign(obj, {});
obj2.name = '小华';
console.log(obj, obj2);
```

- 原对象和复制对象值同时改变，为浅拷贝

3. 扩展运算符

```js
let obj = { name: '小明', function: () => console.log('1'), age: 23, say: { like: '运动' } };

let obj3 = { ...obj };
obj3.name = '小华';
console.log(obj, obj3); // {name: '小明', age: 23, say: {like: '运动'}, function: ƒ}  {name: '小华', age: 23, say: {like: '运动'}, function: ƒ}
obj3.say.like = '打游戏';
console.log(obj, obj3); // {name: '小明', age: 23, say: {like: '打游戏'}, function: ƒ}  {name: '小华', age: 23, say: {like: '打游戏'}, function: ƒ}
```

- 当原对象只有一层的时候，使用以上方法，修改复制对象，不改变原对象，为深拷贝
- 当原对象超过一层的时候，使用以上方法，修改复制对象第二层内的值，会改变原对象第二层内的值，为浅拷贝

4. for/in

```js
let obj = { name: '小明', function: () => console.log('1'), age: 23, say: { like: '运动' } };

let obj4 = {};
for (key in obj) {
  obj4[key] = obj[key];
}

obj4.name = '小华';
console.log(obj, obj4); // {name: '小明', age: 23, say: {like: '运动'}, function: ƒ}  {name: '小华', age: 23, say: {like: '运动'}, function: ƒ}
obj4.say.like = '打游戏';
console.log(obj, obj4); // {name: '小明', age: 23, say: {like: '打游戏'}, function: ƒ}  {name: '小华', age: 23, say: {like: '打游戏'}, function: ƒ}
```

- 当原对象只有一层的时候，使用以上方法，修改复制对象，不改变原对象，为深拷贝
- 当原对象超过一层的时候，使用以上方法，修改复制对象第二层内的值，会改变原对象第二层内的值，为浅拷贝

5. JSON 转化

```js
let obj = { name: '小明', function: () => console.log('1'), age: 23, say: { like: '运动' } };

let obj5 = JSON.parse(JSON.stringify(obj));

obj5.say.like = '打游戏';
console.log(obj, obj5); // {name: '小明', age: 23, say: {like: '运动'}, function: ƒ}  {name: '小明', age: 23, say: {like: '打游戏'}}
```

- 经过 json 转化的复制对象，修改多层不会影响到原对象，但是转化过程会存在丢失对象的问题

6. 递归

```js
let obj = { name: '小明', function: () => console.log('1'), age: 23, say: { like: '运动' } };

const deepClone = (obj, _obj) => {
  Object.keys(obj).map((key) => {
    if (typeof obj[key] === 'object') {
      _obj[key] = {};
      deepClone(obj[key], _obj[key]);
    } else {
      _obj[key] = obj[key];
    }
  });
};
const obj6 = {};
deepClone(obj, obj6);
obj6.say.like = '打游戏';
console.log(obj, obj6); // {name: '小明', age: 23, say: {like:'运动'}, function: ƒ}  {name: '小明', age: 23, say: {like:'打游戏'}, function: ƒ}
```

- 递归遍历复制对象，修改多层不会影响到原对象，也不会丢失对象值
