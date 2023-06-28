---
title: Reflect
date: 2020-6-14
tags: ES6
author: 映雪
---

此情无处可消除，才下眉头，却上心头。

<!--more-->

### 反射机制

- 反射机制指的是程序在运行时能够获取自身的信息。

### Reflect（JS的反射对象）

- Reflect是一个内建的对象，用来提供方法去拦截JavaScript的操作。Reflect不是一个函数对象，所以它是不可构造的，也就是说它不是一个构造器，你不能通过`new`操作符去新建或者将其作为一个函数去调用Reflect对象。Reflect的所有属性和方法都是静态的。

###  Reflect目的

1. 将Object对象的一些明显属于语言内部的方法（比如Object.defineProperty），放到Reflect对象上。现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。也就是说，从Reflect对象上可以拿到语言内部的方法。

2. 修改某些Object方法的返回结果，让其变得更合理。比如，Object.defineProperty(obj, name, desc)在无法定义属性时，会抛出一个错误，而Reflect.defineProperty(obj, name, desc)则会返回false。

```js
// 老写法
try {
  Object.defineProperty(target, property, attributes);
  // success
} catch (e) {
  // failure
}

// 新写法
if (Reflect.defineProperty(target, property, attributes)) {
  // success
} else {
  // failure
}
```

3. 让Object操作都变成函数行为。某些Object操作是命令式，比如name in obj和delete obj[name]，而Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)让它们变成了函数行为。

```js
   const animal = { name:'老虎',age:'11'};
   console.log(animal)
   Reflect.deleteProperty(animal, 'name')
   console.log(Reflect.has(animal,'age'))
   Reflect.set(animal,'sex','男')
   console.log(animal)
```

4. Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。也就是说，不管Proxy怎么修改默认行为，你总可以在Reflect上获取默认行为。

```js
Proxy(target, {
  set: function(target, name, value, receiver) {
    var success = Reflect.set(target, name, value, receiver);
    if (success) {
      console.log('property ' + name + ' on ' + target + ' set to ' + value);
    }
    return success;
  }
});
```

- 上面代码中，Proxy方法拦截target对象的属性赋值行为。它采用Reflect.set方法将值赋值给对象的属性，确保完成原有的行为，然后再部署额外的功能。

### Reflect 静态方法

1. Reflect.get(target, name, receiver) 

- Reflect.get方法查找并返回target对象的name属性，如果没有该属性，则返回undefined。

```js
var myObject = {
  foo: 1,
  bar: 2,
  get baz() {
    return this.foo + this.bar;
  },
};

var myReceiverObject = {
  foo: 4,
  bar: 4,
};
Reflect.get(myObject, 'baz') // 3
Reflect.get(myObject, 'baz', myReceiverObject) // 8
```

- 若name属性部署了读取函数（getter），则读取函数的this绑定receiver。


2. Reflect.set(target, name, value, receiver) 

- Reflect.set方法设置target对象的name属性等于value。

```js
var myObject = {
  foo: 4,
  set bar(value) {
    return this.foo = value;
  },
};

var myReceiverObject = {
  foo: 0,
};
Reflect.set(myObject, 'foo', 2);
myObject.foo // 2

Reflect.set(myObject, 'bar', 1, myReceiverObject);
myObject.foo // 4
myReceiverObject.foo // 1
```

- 若name属性部署了读取函数（getter），则读取函数的this绑定receiver。

- 注意:如果 Proxy对象和 Reflect对象联合使用，前者拦截赋值操作，后者完成赋值的默认行为，而且传入了receiver，那么Reflect.set会触发Proxy.defineProperty拦截。

3. Reflect.has(obj, name) 

- Reflect.has方法对应name in obj里面的in运算符。

```js
var myObject = {
  foo: 1,
};
// 旧写法
'foo' in myObject // true
// 新写法
Reflect.has(myObject, 'foo') // true
```

4. Reflect.deleteProperty(obj, name)

- Reflect.deleteProperty方法等同于delete obj[name]，用于删除对象的属性。

```js
const myObj = { foo: 'bar' };
// 旧写法
delete myObj.foo;
// 新写法
Reflect.deleteProperty(myObj, 'foo');
```

- 该方法返回一个布尔值。如果删除成功，或者被删除的属性不存在，返回true；删除失败，被删除的属性依然存在，返回false。

5. Reflect.construct(target, args) 

- Reflect.construct方法等同于new target(...args)，这提供了一种不使用new，来调用构造函数的方法。

```js
function Greeting(name) {
  this.name = name;
}

// new 的写法
const instance = new Greeting('张三');

// Reflect.construct 的写法
const instance = Reflect.construct(Greeting, ['张三']);
```

6. Reflect.defineProperty(target, propertyKey, attributes)
- Reflect.defineProperty方法基本等同于Object.defineProperty，用来为对象定义属性。未来，后者会被逐渐废除，请从现在开始就使用Reflect.defineProperty代替它。

```js
function MyDate() {
  /*…*/
}
// 旧写法
Object.defineProperty(MyDate, 'now', {
  value: () => Date.now()
});
// 新写法
Reflect.defineProperty(MyDate, 'now', {
  value: () => Date.now()
});
```

7. Reflect.getPrototypeOf(obj)

- Reflect.getPrototypeOf方法用于读取对象的__proto__属性，对应Object.getPrototypeOf(obj)。

```js
const myObj = new FancyThing();
// 旧写法
Object.getPrototypeOf(myObj) === FancyThing.prototype;
// 新写法
Reflect.getPrototypeOf(myObj) === FancyThing.prototype;
```