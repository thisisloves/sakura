---
title: Class继承
date: 2019-3-23
tags: ES6
author: 映雪
---

我读过一本书，书上说这个世界上有两万个人是会跟你一见钟情的，可惜终你一生都未必能遇见他们中的任何一个。一见钟情不是个魔法，它是命运。

<!--more-->

### Class

- 直接使用原型对象模仿面向对象中的类和类继承,JS 中并没有一个真正的 class 原始类型， class 仅仅只是对原型对象运用语法糖。

```js
function Person(name) {
  this.name = name;
  this.age = 18;
  this.sex = '男';
}
Person.prototype.say = function () {
  console.log('这是构造函数原型对象的方法');
};
```

- class 写法

```js
class Person {
  constructor(name) {
    this.name = name;
    this.age = 18;
    this.sex = '男';
  }
  say() {
    console.log('这是构造函数原型对象的方法');
  }
}
```

### Class 继承

- 和 ES5 继承区别

- ES5 的继承，实质是先创造子类的实例对象 this，然后再将父类的方法添加到 this 上（Parent.apply(this)）
- ES6 的继承机制完全不同，实质是先创造父类的实例对象 this（所以必须先调用 super 方法）,然后再用子类的构造函数修改 this；

```js
class Person {
  static age = 23;
  constructor(name) {
    this.name = name;
    this.sex = '男';
    this.fn = function () {
      console.log(this);
    }.bind(this);
  }
  say() {
    console.log('这是父原型对象的方法', this);
  }
  static speak() {
    console.log('这是父的静态方法,可直接调用', this);
  }
}
const person = new Person('小华');
Person.speak(); // 这是父的静态方法,可直接调用  class Person{...}
console.log(Person.age); // 23

class Child extends Person {
  static num = 12;
  constructor(name, like) {
    super(name); // 调用父级的constructor
    this.like = like;
  }
  childSay() {
    console.log('这是子的原型对象的方法', this);
  }
  static childSpeak() {
    console.log('这是子的静态方法,可直接调用', this);
  }
}
const child = new Child('小华', '打游戏');
console.log(child); // Child {name: '小华', sex: '男', like: '打游戏', fn: ƒ}
child.say(); // 这是父原型对象的方法 Child {name: '小华', sex: '男', like: '打游戏', fn: ƒ}
child.childSay(); // 这是子的原型对象的方法 Child {name: '小华', sex: '男', like: '打游戏', fn: ƒ}
Child.speak(); // 这是父的静态方法,可直接调用 class Child extends Person {...}
Child.childSpeak(); // 这是子的静态方法,可直接调用 class Child extends Person {...}
console.log(Child.age); // 23
console.log(Child.num); // 13
```

- 注意：子类必须在 constructor()方法中调用 super()，否则就会报错。

1. 子类自己的 this 对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，添加子类自己的实例属性和方法。如果不调用 super()方法，子类就得不到自己的 this 对象。
2. ES5 的继承机制，是先创造一个独立的子类的实例对象，然后再将父类的方法添加到这个对象上面，即“实例在前，继承在后”。
3. ES6 的继承机制，则是先将父类的属性和方法，加到一个空的对象上面，然后再将该对象作为子类的实例，即“继承在前，实例在后”。这就是为什么 ES6 的继承必须先调用 super()方法，因为这一步会生成一个继承父类的 this 对象，没有这一步就无法继承父类。
4. 新建子类实例时，父类的构造函数必定会先运行一次。

### super 关键字

- super 这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。

1. 作为函数调用

- 代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次 super 函数。

```js
class Person {
  static age = 23;
  constructor(name) {
    this.name = name;
    this.sex = '男';
    this.fn = function () {
      console.log(this);
    }.bind(this);
  }
  say() {
    console.log('这是父原型对象的方法', this);
  }
  static speak() {
    console.log('这是父的静态方法,可直接调用', this);
  }
}

class Child extends Person {
  static num = 12;
  constructor(name, like) {
    super(name); // 调用父级的constructor
    this.like = like;
  }
}
```

- super 虽然代表了父类的构造函数，但是返回的是子类的实例，即 super 内部的 this 指的是子类的实例，因此 super()相当于父类.prototype.constructor.call(this)。

- super()内部的 this 指向的是子类。

2. super 作为对象使用

- 在普通方法中，指向父类的原型对象；在静态方法中，指向父类。

```js
class Person {
  static age = 23;
  constructor(name) {
    this.name = name;
    this.sex = '男';
    this.fn = function () {
      console.log(this);
    }.bind(this);
  }
  say() {
    console.log('这是父原型对象的方法', this);
  }
  static speak() {
    console.log('这是父的静态方法,可直接调用', this);
  }
}

class Child extends Person {
  static num = 12;
  constructor(name, like) {
    super(name); // 调用父级的constructor
    this.like = like;
  }
  childSay() {
    console.log(super.sex);  // undefined
    console.log(super.say);  // f say(){...}
  }
  static childSpeak() {
    console.log(super.age);    // 23
    console.log(super.speak);  // f speak(){...}
  }
}
```

- super 指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过 super 调用的。
- 在子类普通方法中通过super调用父类的方法时，方法内部的this指向当前的子类实例。
- 在静态方法通过super调用父类的方法时，这时super将指向父类，而不是父类的原型对象。


### 类的 prototype 属性和__proto__属性

1. 子类的__proto__属性，表示构造函数的继承，总是指向父类。

2. 子类prototype属性的__proto__属性，表示方法的继承，总是指向父类的prototype属性。
