---
title: Set数据结构
date: 2018-3-18
tags: ES6
author: 映雪
---

若是一枝梅花，清明丝雨泠泠洒，折一枝，泣露白梅花。

<!--more-->

### Set

- Set 本身是一个构造函数，用来生成 Set 数据结构,**类似于数组**，但是成员的值都是唯一的，没有重复的值。
- Set 函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。

```js
const set = new Set([3, 4, 5, 6, 3]);
const div = new Set(document.querySelectorAll('.name'));

console.log(set, div);
```

### Set 实例的属性

1. Set.prototype.constructor：构造函数，默认就是 Set 函数。
2. Set.prototype.size：返回 Set 实例的成员总数。

```js
const set = new Set([3, 4, 5, 6, 3]);
const div = new Set(document.querySelectorAll('.name'));

console.log(set.size); // 4
```

### Set 实例的方法

1. Set.prototype.add(value)

- 添加某个值,返回 Set 结构本身

2. Set.prototype.delete(value)

- 删除某个值，返回一个布尔值，表示删除是否成功。

3. Set.prototype.has(value)

- 返回一个布尔值，表示该值是否为 Set 的成员。

4. Set.prototype.clear()

- 清除所有成员，没有返回值。

```js
const set = new Set([0, 3, 4, 4, document.querySelectorAll('.name')]);
console.log(set);
set.add({ name: '小华' });
console.log(set);
set.delete(0);
console.log(set.has(4));
```

![截屏2022-01-18 上午10.43.07.png](/images/2022/01/18/LQgYwEbDfFadke1.png)

### Set 遍历

1. Set.prototype.keys()

- 返回键名的遍历器

2. Set.prototype.values()

- 返回键值的遍历器

3. Set.prototype.entries()

- 返回键值对的遍历器

4. Set.prototype.forEach()：

- 使用回调函数遍历每个成员

```js
const set = new Set([0, 3, 4, 4, document.querySelectorAll('.name')], { name: '小华' });
console.log(set);
for (let item of set.keys()) {
  console.log(item);
}
for (let item of set.values()) {
  console.log(item);
}
for (let item of set.entries()) {
  console.log(item);
}
set.forEach((item, key) => {
  console.log(item, key);
});
```

![截屏2022-01-17 下午6.19.58.png](/images/2022/01/17/b7OdEmHhGLYePxj.png)

### 改变 Set 结构

```js
// 方法一
let set = new Set([1, 2, 3]);
set = new Set([...set].map((val) => val * 2));
// set的值是2, 4, 6

// 方法二
let set = new Set([1, 2, 3]);
set = new Set(Array.from(set, (val) => val * 2));
// set的值是2, 4, 6
```


### WeakSet 和 Set 区别

- 1. WeakSet 的成员只能是对象，而不能是其他类型的值。
- 2. WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。