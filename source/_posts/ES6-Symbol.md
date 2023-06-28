---
title: Symbol
date: 2018-12-30
tags: ES6
author: 映雪
---

始于初见，止于终老。

<!--more-->

### Symbol

- 新的原始数据类型 Symbol，表示独一无二的值, 用于确保对象的属性不重复。
- ES5 对象属性名都是字符串，这容易造成属性名的冲突。为对象添加新的方法（mixin 模式），新方法的名字就有可能与现有方法产生冲突。Symbol 保证每个属性的名字都是独一无二，从根本上防止属性名的冲突。
- Symbol 值通过 Symbol 函数生成。对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。

### Symbol 使用

```js
const a = Symbol();
const b = Symbol();
console.log(a); //Symbol()
console.log(b); //Symbol()
```

![截屏2022-01-19 下午6.02.20.png](/images/2022/01/19/hwQeJZt59fiNFLs.png)

- Symbol.prototype.description
- 创建Symbol 的时候可以添加一个描述

```js
const a = Symbol('这是描述');
console.log(a) // Symbol(这是描述)
```

### Symbol 赋值

```js
const a = Symbol();
const b = Symbol();
const c = a
console.log(a); //Symbol()
console.log(b); //Symbol()
console.log(c); //Symbol()
```

![截屏2022-01-19 下午6.16.03.png](/images/2022/01/19/YlBxUZDFGXoA2Iw.png)

### 属性名 Symbol

```js
  let mySymbol = Symbol('小华')
  let mySymbolKey  = Symbol('key')
  let obj ={
      name:'小明',
      [mySymbolKey]:'这是值'
  }
  obj[mySymbol] = '小华';
  console.log(obj,obj[mySymbol])
```

### 属性名遍历

- Symbol 作为属性名，遍历对象的时候，该属性不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回。

- 但该属性并不是私有属性，它可以被专门的Object.getOwnPropertySymbols()方法遍历出来。该方法返回一个数组，包含了当前对象的所有用作属性名的Symbol值

```js
  let mySymbol = Symbol('小华')
  let mySymbolKey  = Symbol('key')
  let obj ={
      name:'小明',
      [mySymbolKey]:'这是值'
  }
  obj[mySymbol] = '小华';
 
  const arrSymbol = Object.getOwnPropertySymbols(obj);
  console.log(arrSymbol)            // [Symbol(key), Symbol(小华)]
  console.log(obj[arrSymbol[0]])    // 这是值
  for(let item in obj){
      console.log(item)  // name
  }
```

- Reflect.ownKeys()
- 可以遍历出所有的常规键名和Symbol键名。

```js
 console.log( Reflect.ownKeys(obj))   // ['name', Symbol(key), Symbol(小华)]
```

### Symbol.for()

- 接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建一个以该字符串为名称的 Symbol 值，并将其注册到全局。
- Symbol.for()与Symbol()这两种写法，都会生成新的 Symbol。它们的区别是，前者会被登记在全局环境中供搜索，后者不会。
- Symbol.for("cat")30 次，每次都会返回同一个 Symbol 值，但是调用Symbol("cat")30 次，会返回 30 个不同的 Symbol 值。

```js
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

s1 === s2 // true
```

### Symbol.keyFor()

- 返回一个已登记的 Symbol 类型值的key。

```js
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

### 内置的 Symbol

1. Symbol.hasInstance

- 当使用instanceof运算符判断某个对象是否为某个构造函数的实例时，就是在调用该构造函数上的静态方法[Symbol.hasInstance]。

```js
[] instanceof Array;  //true

//浏览器实际上是在调用下面的方法
Array[Symbol.hasInstance]([]);
```

2. Symbol.isConcatSpreadable

- 当前对象作为concat的参数时是否可以展开。
- obj被传入concat后会直接作为一个元素添加到数组中。通过将obj的Symbol.isConcatSpreadable属性设置为true，obj会在执行concat时尝试展开，如果该对象无法展开，obj不会被拼接到数组中去。所谓的可展开，指的是obj是否为数组或类数组结构。如果obj是数组，显然是可展开的，如果它有length属性，并且有"0"，"1"这样的属性键，那么它就是类数组，也是可以展开的。

```js
 let obj ={ length: 2, 0:'我是一',1:'我是二',name:'小芳',[Symbol.isConcatSpreadable]:true}
 console.log([].concat(obj))  // ['我是一','我是二']
```

3. Symbol.match/replace/search/split

- 以对象的方式 自定义 String的match、replace、search、split方法。

```js
let obj ={
    name:'小华',
    nameArr:["小华","小明"],
    [Symbol.match](value){ return obj.name.match(value)[0] },
    [Symbol.replace](value){ return value.replace(obj.name) },
    [Symbol.filter](value){ return obj.nameArr.filter(val=>val === value)[0] },
    [Symbol.search](value){ return obj.name.search(value) }
};

console.log(obj[Symbol.match]("小")) // 小
console.log(obj[Symbol.replace]("小"))  // 小
console.log(obj[Symbol.filter]('小明'))  // 小明
console.log(obj[Symbol.search]('华'))   // 1

```

4. Symbol.iterator 

- 对象的Symbol.iterator属性，指向该对象的默认遍历器方法。
- 凡是具有[Symbol.iterator]方法的对象都是可遍历的，可以使用for of循环依次输出对象的每个属性。
- 数组和类数组以及set数据结构，map数据结构内置该方法。

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