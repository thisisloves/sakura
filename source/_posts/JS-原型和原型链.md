---
title: 原型与原型链
date: 2019-1-29
tags: JS
author: 映雪
---

人生若只如初见，何须感伤离别。

<!--more-->

### 前言

- ES6 之前中并没有引入类（class）的概念，JavaScript 并非通过类而是直接通过构造函数来创建实例


### 普通对象与函数对象

- 通过 new Function() 创建的对象都是函数对象，其他的都是普通对象。
- Array 、Object 、Function 是 JS 自带的函数对象。

```js
const a = new Function('1')
const b = function(){}
const c = ()=>{}
const d = new function Aa(){}

console.log(aa)
console.log(bb)
console.log(cc)
console.log(dd)

console.log(typeof a)   // Function
console.log(typeof b)   // Function
console.log(typeof c)   // Function
console.log(typeof d)   // Objetct
 
console.log(typeof Array)     // Function
console.log(typeof Object)    // Function
console.log(typeof Function)  // Function
```


### 构造函数

- 构造函数模式的目的就是为了创建一个自定义类，并且创建这个类的实例。
- 构造函数就是一个普通的函数，创建方式和普通函数没有区别，不同的是构造函数习惯上首字母大写。另外就是调用方式的不同，普通函数是直接调用，而构造函数需要使用 new 关键字来调用。


```js
 function Person (name,sex,age){
    this.name = name;
    this.sex = sex;
    this.age = age;
    console.log('我的名字是'+name+',我的性别是'+sex+',我今年'+age+'岁')
    this.fn = function(){
        console.log('这是Person的方法')
    }
}
const person = new Person('小明','男','23');
person.fn()   // 这是Person的方法
console.log(person.__proto__.constructor == Person)  // true

```

- person是Person的实例对象，实例对象通过`__proto__`可以访问构造函数的原型对象


### 原型

- 在 JavaScript 中，每当定义一个函数数据类型(普通函数、类)时候，都会天生自带一个 prototype 属性，这个属性指向函数的原型对象，并且这个属性是一个对象数据类型的值。
- 每个对象都有 `__proto__ `属性，但只有函数对象才有 prototype 属性。

```js
function Person (name,sex,age){
    this.name = name;
    this.sex = sex;
    this.age = age;
    console.log('我的名字是'+name+',我的性别是'+sex+',我今年'+age+'岁')
    this.fn = function(){
        console.log('这是Person的方法')
    }
}
Person.prototype.say = function (){'这是Person的原型方法' }

const person = new Person('小明','男','23');
const person2 = new Person('小华','女','25');

console.log(person)  // Person {name: '小明', sex: '男', age: '23', fn: ƒ}
console.log(person2) // Person {name: '小华', sex: '女', age: '25', fn: ƒ}



console.log(person.prototype)   //  undefined  无 prototype
console.log(person2.prototype)  //  undefined  无 prototype
console.log(Person.prototype)   //  {say: ƒ, constructor: ƒ}
console.log(person.__proto__)   //  {say: ƒ, constructor: ƒ}
console.log(person2.__proto__)   //  {say: ƒ, constructor: ƒ}

console.log(Person.prototype.constructor == Person)  // true
console.log(person.constructor  ==  Person)  // true
console.log(person2.constructor  ==  Person)  // true
console.log(person.__proto__.constructor == Person)  // true
console.log(person2.__proto__.constructor == Person)  // true


console.log(Person.prototype.say )   // ƒ (){'这是Person的原型方法' }
console.log(person.say )  // ƒ (){'这是Person的原型方法' }
console.log(person2.say)  // ƒ (){'这是Person的原型方法' }
console.log(person.__proto__.say )  // ƒ (){'这是Person的原型方法' }
console.log(person2.__proto__.say)  // ƒ (){'这是Person的原型方法' }
```

- 构造函数和实例原型之间的关系

![截屏2021-05-07下午6.13.28.png](/images/2021/05/07/JZUihrRLVklD3bX.png)

- 原型对象就相当于一个公共的区域，所有同一个类的实例都可以访问到这个原型对象，我们可以将对象中共有的内容，统一设置到原型对象中。

### 原型链

1.  `__proto__`

- 每一个对象数据类型(普通的对象、实例、prototype......除了null)也天生自带一个属性`__proto__`，属性值是当前实例所属类的原型(prototype)

```js
function Person (name,sex,age){
    this.name = name;
    this.sex = sex;
    this.age = age;
    console.log('我的名字是'+name+',我的性别是'+sex+',我今年'+age+'岁')
    this.fn = function(){
        console.log('这是Person的方法')
    }
}
Person.prototype.say = function (){'这是Person的原型方法' }

const person = new Person('小明','男','23');
const person2 = new Person('小华','女','25');

console.log(person.__proto__)   //  {say: ƒ, constructor: ƒ}
console.log(person2.__proto__)   //  {say: ƒ, constructor: ƒ}
console.log(person.__proto__ == Person.prototype)  // true
console.log(person2.__proto__ == Person.prototype)  // true
```


2. constructor

- 原型对象中有一个属性 constructor，它指向函数对象。


```js
function Person (name,sex,age){
    this.name = name;
    this.sex = sex;
    this.age = age;
    console.log('我的名字是'+name+',我的性别是'+sex+',我今年'+age+'岁')
    this.fn = function(){
        console.log('这是Person的方法')
    }
}
Person.prototype.say = function (){'这是Person的原型方法' }

const person = new Person('小明','男','23');
const person2 = new Person('小华','女','25');

console.log(Person.prototype.constructor == Person)  // true
console.log(person.constructor  ==  Person)  // true
console.log(person2.constructor  ==  Person)  // true
console.log(person.__proto__.constructor == Person)  // true
console.log(person2.__proto__.constructor == Person)  // true
```

![截屏2021-05-07下午6.22.45.png](/images/2021/05/07/VEAxsWS7Hi9Kwuy.png)


### 原型链理解

- 万物都是对象，对象和对象之间也有关系，并不是孤立存在的。对象之间的继承关系，在JavaScript中是通过prototype对象指向父类对象，直到指向Object对象为止，这样就形成了一个原型指向的链条，称之为原型链。

- 当我们访问对象的一个属性或者方法时候，会在对象自身中寻找，如果有就直接调用，如果没有则会去原型对象中寻找，如果找到则直接使用。如果没有则去原型链的原型中寻找，直到找到Object对象的原型，Object对象的原型没有原型，如果中Object 原型中依然没有找到，则返回undefined。

![截屏2022-02-24 下午5.01.30.png](/images/2022/02/24/OkMuioICHtc9dlB.png)


### 原型链顶层

-  JS中万物皆对象，所有内容的指针终点都指向 Object。
-  原型链的顶层就是Object.prototype，而这个对象的是没有原型对象的。

```js
 function Person (name,sex,age){
    this.name = name;
    this.sex = sex;
    this.age = age;
    console.log('我的名字是'+name+',我的性别是'+sex+',我今年'+age+'岁')
    this.fn = function(){
        console.log('这是Person的方法')
    }
}
Person.prototype.say = function (){'这是Person的原型方法' }

console.log(Object.prototype)
console.log(Person.prototype)
```

![截屏2022-02-25 上午10.18.26.png](/images/2022/02/25/a2eNzgfyuDnchdB.png)
