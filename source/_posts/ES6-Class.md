---
title: Class
date: 2019-3-21
tags: ES6
author: 映雪
---

所谓爱情，就是你总会遇到一个对的人，他让你感觉生活越来越美好，而不是再黏着过去不肯放手。

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

### Class 语法

- 注意点

1. 类的所有方法都定义在类的 prototype 属性上面，在类的实例上面调用方法，其实就是调用原型上的方法。
2. 类中声明的方法不能加 function 关键字。
3. 方法之间不需要逗号分隔，加了会报错。
4. ES6 的 class 使用方法与 ES5 的构造函数一模一样。
5. constructor 是一个构造函数方法，创建对象时自动调用该方法。
6. 不能像普通函数一样调用。

- class 属性

1. 实例属性：constructor 里面的属性为实例属性，即定义在 this 对象上。
2. 原型属性：除去实例属性都称为原型属性，即定义在 class 类上。

```js
function Person1(name) {
  this.name = name;
  this.age = 18;
  this.sex = '男';
  this.function = function speak() {
    console.log('这是构造函数的方法', this);
  };
}
Person1.prototype.say = function () {
  console.log('这是构造函数原型对象的方法', this);
};

const person1 = new Person1('小华');
console.log(person1); // Person1 {name: '小华', age: 18, sex: '男', function: ƒ}
person1.say(); // 这是构造函数原型对象的方法   Person1 {name: '小华', age: 18, sex: '男', function: ƒ}
person1.function(); // 这是构造函数的方法 Person1 {name: '小华', age: 18, sex: '男', function: ƒ}

class Person {
  constructor(name) {
    this.name = name;
    this.age = 18;
    this.sex = '男';
    this.function = function speak() {
      console.log('这是构造函数的方法', this);
    };
  }
  say() {
    console.log('这是构造函数原型对象的方法', this);
  }
}

const person = new Person('小华');
console.log(person1); // Person {name: '小华', age: 18, sex: '男', function: ƒ}
person1.say(); // 这是构造函数原型对象的方法   Person {name: '小华', age: 18, sex: '男', function: ƒ}
person1.function(); // 这是构造函数的方法 Person {name: '小华', age: 18, sex: '男', function: ƒ}
```

### Constructor

- constructor()方法是类的默认方法，通过 new 命令生成对象实例时，自动调用该方法。一个类必须有 constructor()方法，如果没有显式定义，一个空的 constructor()方法会被默认添加。

- constructor 方法默认返回实例对象（即 this），可以指定返回另外一个对象。

```js
const obj = {
  name: '小明',
  age: 22,
};
class Person {
  constructor(name) {
    this.name = name;
    this.age = 18;
    this.sex = '男';
    this.function = function speak() {
      console.log('这是构造函数的方法', this);
    };
    return obj;
  }
  say() {
    console.log('这是构造函数原型对象的方法', this);
  }
}

const person = new Person('小华');
console.log(person); // {name: '小明', age: 22}
```

### getter 和 setter

- 在“类”的内部可以使用 get 和 set 关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
    this.sex = '男';
    this.function = function speak() {
      console.log('这是构造函数的方法', this);
    };
  }
  get age() {
    return this.age;
  }
  set age(value) {
    console.log(value, this);
  }
  say() {
    console.log('这是构造函数原型对象的方法', this);
  }
}

const person = new Person('小华', 12); // 12  Person {name: '小华', sex: '男', function: ƒ}
person.age = 13; // 13  Person {name: '小华', sex: '男', function: ƒ}
```

### Class 表达式

- 与函数一样，类也可以使用表达式的形式定义。

```js
  const per =  class Person {
      constructor(name,age) {
          this.name = name
          this.age = age
          this.sex = "男"
          this.function = function speak(){  console.log('这是构造函数的方法',this)}
      }
      say() {
          console.log('这是构造函数原型对象的方法',this)
      }
  }

  const person = new per('小华',12)
  console.log(person)   // Person {name: '小华', age: 12, sex: '男', function: ƒ}
```

### Class 注意

1. 严格模式

- 类和模块的内部，默认就是严格模式，所以不需要使用use strict指定运行模式。

2. 不存在提升

- ES6 不会把类的声明提升到代码头。

3. this 的指向

- 类的方法内部如果含有this，它默认指向类的实例。一旦单独使用该方法，很可能报错。this会指向该方法运行时所在的环境（由于 class 内部是严格模式，所以 this 实际指向的是undefined），从而导致找不到print方法而报错。

```js
   const person =   new class Person  {
        constructor(name){
            this.name = name
            this.fn = function(){console.log(this)}
        }
        say() {
            console.log('这是构造函数原型对象的方法',this)
        }
    }
    person.name= '小华'
    console.log(person)   // Person {name: '小华', fn: ƒ}
    person.fn()   // Person {name: '小华', fn: ƒ}
    const {fn} = person
    console.log(fn)  // function(){console.log(this)}
    fn()   // undefined 
```


- 解决方法

1. 在构造方法中绑定this

```js
  const person =   new class Person  {
      constructor(name){
          this.name = name
          this.fn = function(){console.log(this)}.bind(this);
      }
      say() {
          console.log('这是构造函数原型对象的方法',this)
      }
  }
  person.name= '小华'
  console.log(person) // Person {name: '小华', fn: ƒ}
  person.fn() // Person {name: '小华', fn: ƒ}
  const {fn} = person
  console.log(fn)  // ()=>console.log(this)
  fn()    //  Person {name: '小华', fn: ƒ}
```

2. 使用箭头函数

- 自动绑定词法作用域

```js
   const person =   new class Person  {
        constructor(name){
            this.name = name
            this.fn =()=>console.log(this)
        }
        say() {
            console.log('这是构造函数原型对象的方法',this)
        }
    }
    person.name= '小华'
    console.log(person) // Person {name: '小华', fn: ƒ}
    person.fn() // Person {name: '小华', fn: ƒ}
    const {fn} = person
    console.log(fn)  // ()=>console.log(this)
    fn()    //  Person {name: '小华', fn: ƒ}
```

### 静态方法

- 类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```js
  class Person  {
    constructor(name){
        this.name = name
        this.fn = function(){console.log(this)}.bind(this);
    }
    static speak (){
        console.log('这是静态方法,可直接调用',this)
    }
    say() {
        console.log('这是构造函数原型对象的方法',this)
    }
}
const person = new Person('小华')
person.say()  // 这是构造函数原型对象的方法 Person {name: '小华', fn: ƒ}
Person.speak()  // 这是静态方法,可直接调用 class Person {...}
```

- 注意:如果静态方法包含this关键字，这个this指的是类，而不是实例。


### 静态属性

```js
  class Person  {
    static age = 23
    constructor(name){
        this.name = name
        this.fn = function(){console.log(this)}.bind(this);
    }
    static speak (){
        console.log('这是静态方法,可直接调用',this)
    }
    say() {
        console.log('这是构造函数原型对象的方法',this)
    }
}
const person = new Person('小华')
Person.speak() // 这是静态方法,可直接调用 class Person {...}
console.log(Person.age)  // 23
```