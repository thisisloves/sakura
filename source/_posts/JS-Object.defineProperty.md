---
title: Object.defineProperty
date: 2018-05-15
tags: JS
author: 映雪
---

夜冷梦长,长不过人海茫茫;人走茶凉,凉不过青丝成霜。

<!--more-->

### Object.defineProperty()

- Object.defineProperty()的作用就是直接在一个对象上定义一个新属性，或者修改一个已经存在的属性。
- 正常对象属性赋值，对象的属性可以修改可以删除，但是通过 Object.definedProperty()定义属性，可以更精确的控制对象属性。

```js
Object.defineProperty(obj, prop, desc);
```

1. obj 需要定义属性的当前对象
2. prop 当前需要定义的属性名
3. desc 属性描述符

### 属性类型

- JavaScript 有三种属性类型

1. 命名数据属性：拥有一个确定的值的属性。这也是最常见的属性
2. 命名访问器属性：通过 getter 和 setter 进行读取和赋值的属性
3. 内部属性：由 JavaScript 引擎内部使用的属性，不能通过 JavaScript 代码直接访问到，不过可以通过一些方法间接的读取和设置。比如，每个对象都有一个内部属性[[Prototype]]，你不能直接访问这个属性，但可以通过 Object.getPrototypeOf()方法间接的读取到它的值。

### 属性描述符

- 通过 Object.defineProperty()为对象定义属性，有两种形式，且不能混合使用，分别为数据描述符，存取描述符。

|            | configurable | enumerable | value  | writable |  get   |  set   |
| ---------- | :----------: | :--------: | :----: | :------: | :----: | :----: |
| 数据描述符 |     可以     |    可以    |  可以  |   可以   | 不可以 | 不可以 |
| 存取描述符 |     可以     |    可以    | 不可以 |  不可以  |  可以  |  可以  |

1. 数据描述符

- 据描述符是一个具有值的属性，该值可以是可写的，也可以是不可写的。

```js
let Person = {};
// 修改对象 Person 默认属性 特征
Object.defineProperty(Person, 'name', {
  value: 'jack',
  writable: true, // 是否可以改变
  configurable: true, // 描述属性是否可被配置
  enumerable: true, // 访问器属性是否可被遍历
});
console.log(Person.name);

Person.name = 'rose';
console.log(Person.name);
```

2. 存取描述符

- 是由一对 getter、setter 函数功能来描述的属性。

- get：一个给属性提供 getter 的方法，如果没有 getter 则为 undefined。该方法返回值被用作属性值。默认为 undefined。
- set：一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。该方法将接受唯一参数，并将该参数的新值分配给该属性。默认值为 undefined。

```js
let Book = { name: '' };
let tem = Book.name;
Object.defineProperty(Book, 'name', {
  get: function () {
    return tem;
  },
  set: function (val) {
    console.log('我修改了对象');
    tem = val;
  },
});
console.log(Book.name);
Book.name = '平凡的世界';
console.log(Book.name);
```

### 描述符

|    描述符    |                                                                                 描述                                                                                 |   默认    |
| :----------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :-------: |
| configurable |                          当且仅当该属性的 `configurable` 键值为 `true` 时，该属性的描述符才能够被改变，同时该属性也能从对应的对象上被删除。                          |   false   |
|  enumerable  |                                          当且仅当该属性的 `enumerable` 键值为 `true` 时，该属性才会出现在对象的枚举属性中。                                          |   false   |
|    value     |                                                该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。                                                | undefined |
|   writable   |                                  当且仅当该属性的 `writable` 键值为 `true` 时，属性的值，也就是上面的 `value`，才能被赋值运算符改变                                  | undefined |
|     get      | 当访问该属性时，会调用此函数。执行时不传入任何参数，但是会传入 `this` 对象（由于继承关系，这里的`this`并不一定是定义该属性的对象）。该函数的返回值会被用作属性的值。 | undefined |
|     set      |                                当属性值被修改时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 `this` 对象。                                | undefined |



### Object.defineProperty()与 Proxy 区别

1. Object.defineProperty()只能监听某个属性不能全对象监听。
2. Proxy去代理，他会返回一个新的代理对象不会对原对象进行改动，而Object.defineProperty()是去修改原对象，修改原对象的属性，而proxy只是对原对象进行代理并给出一个新的代理对象
3. Proxy可以直接监听数组的变化。
4. Proxy有多达13种拦截方法,不限于apply、ownKeys、deleteProperty、has等等是Object.defineProperty不具备的。