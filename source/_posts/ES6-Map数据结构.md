---
title: Map数据结构
date: 2018-4-18
tags: ES6
author: 映雪
---

拂过红尘衣袂，涤荡污浊俗心。 愿纵马而歌归故里，能得你素衣不改，白裙曳地，夹道相迎。

<!--more-->

### Map

- Map 本身是一个构造函数，用来生成 Map 数据结构，Map数据结构 **类似于对象**，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。
- Map 也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组。

```js
const items = [
  ['name', '张三'],
  ['title', 'Author']
];

const map = new Map();

items.forEach(
  ([key, value]) => map.set(key, value)
);

console.log(map)
```

### Map 实例的属性

1. Map.prototype.constructor：构造函数，默认就是 Map 函数。
2. Map.prototype.size：返回 Map 实例的成员总数。

```js
const map = new Map([[{name:'小华'},'爱吃糖']]);
console.log(map.size); // 1
```

### Map 实例的方法

1. Map.prototype.set(key, value)

- set方法设置键名key对应的键值为value，然后返回整个 Map 结构。如果key已经有值，则键值会被更新，否则就新生成该键。

2. Map.prototype.get(key)

- get方法读取key对应的键值，如果找不到key，返回undefined。

3. Map.prototype.has(key)

- has方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。

4. Map.prototype.delete(key)

- delete方法删除某个键，返回true。如果删除失败，返回false。

5. Map.prototype.clear()

- clear方法清除所有成员，没有返回值。


```js
const map = new Map([[{name:'小华'},'爱吃糖']]);
map.set(1,'我是值');
map.set(undefined,'我是undefined');
console.log(map.get(1))
console.log(map.get({name:'小华'}))
console.log(map.get(undefined))
console.log(map.has(1))
map.delete(undefined)
console.log(map.get(undefined))  
```
![截屏2022-01-18 上午10.40.55.png](/images/2022/01/18/Fz4HOeUd9CxGAQo.png)

### Map 遍历

1. Map.prototype.keys()

- 返回键名的遍历器

2. Map.prototype.values()

- 返回键值的遍历器

3. Map.prototype.entries()

- 返回所有成员的遍历器

4. Map.prototype.forEach()：

- 遍历 Map 的所有成员

```js
const map = new Map([[{name:'小华'},'爱吃糖'],[undefined,'我是undefined'],[1,'我是值']]);
for(let item of map.keys()){
    console.log(item)
}
for(let item of map.values()){
    console.log(item)
}
for(let item of map.entries()){
    console.log(item)
}
map.forEach((item,key) => {
    console.log(item,key)
});
```

![截屏2022-01-18 上午10.53.07.png](/images/2022/01/18/3GDpS7hIFcMKUbT.png)

### 改变 Map 结构

1.Map 转数组

```js
const map = new Map([[{name:'小华'},'爱吃糖'],[undefined,'我是undefined'],[1,'我是值']]);
console.log([...map])
```
2. Map 转对象

```js
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

3.对象转 Map

```js
let obj = {"a":1, "b":2};
let map = new Map(Object.entries(obj));
```

### WeakMap 和 Map 区别

- 1. WeakMap只接受对象作为键名（null除外），不接受其他类型的值作为键名。
- 2. WeakMap的键名所指向的对象，不计入垃圾回收机制。它的键名所引用的对象都是弱引用，即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。



