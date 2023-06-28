---
title: JS继承
date: 2019-2-10
tags: JS
author: 映雪
---

人世间有百媚千红，唯独你是我情之所钟。

<!--more-->

### 构造函数和普通函数

- 构造函数，是用来创建对象的函数，本质上也是函数。与其他函数的区别在于调用方式不同，如果通过new操作符来调用的，就是构造函数，如果没有通过new操作符来调用的，就是普通函数。

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
Person('小光','男',15) 

console.log(name,sex,age)  // 小光,男,15
console.log(person)  // Person {name: '小明', sex: '男', age: '23', fn: ƒ}
```

-  当做普通函数调用，这里相当于给window对象添加了name和age属性和sex属性，调用方式不同，产生的效果不同。


### 继承

- A对象通过继承 B 对象，就能直接拥有 B 对象的所有属性和方法。


### 继承方式

1. **原型链继承**

- 让新的实例的原型等于父类的实例

```js
function Parent (name){
    this.name = name?name:'小华';
    this.sex = '女';
    this.age = 16;
    this.parentFn = function(){
        console.log('我的名字是'+this.name+',我的性别是'+this.sex+',我今年'+this.age+'岁')
    }
}
Parent.prototype.say = function(){console.log('我是构造函数原型对象的方法')}

function Child (){
    this.childFn=function (){
        console.log('这是子的方法')
    }
}

Child.prototype = new Parent;
Child.prototype.speak = function(){console.log('我是子函数原型对象的方法')}
const child = new Child();

child.say()   // 我是构造函数原型对象的方法
child.speak() // 我是子函数原型对象的方法
child.childFn() // 这是子的方法
child.parentFn() // 我的名字是小华,我的性别是女,我今年16岁

```


- 特点：
1. 实例可继承的属性有：父类的构造函数的属性，实例的构造函数的属性，父类的原型的实例。

- 缺点：
1. 新实例无法向构造函数传递参数。
2. 继承单一。
3. 所有的新实例都会共享父类实例的属性。（原型上的属性是共享的，一个实例修改类原型属性，另一个实例的原型属性也会被改变）



2. **构造函数继承**

- 用.call()和.apply()将父类构造函数引入子类函数（在子类函数中做了父类函数的自执行（复制））

```js
function Parent (name){
    this.name = name?name:'小华';
    this.sex = '女';
    this.age = 16;
    this.parentFn = function(){
        console.log('我的名字是'+this.name+',我的性别是'+this.sex+',我今年'+this.age+'岁')
    }
}
Parent.prototype.say = function(){console.log('我是构造函数原型对象的方法')}

function Child2 (){
    Parent.call(this,'小明')
    this.childFn=function (){
        console.log('这是子的方法')
    }
}
Child2.prototype.speak = function(){console.log('我是子函数原型对象的方法')}

const child2 = new Child2();
child2.childFn()  // 这是子的方法
child2.parentFn() // 我的名字是小明,我的性别是女,我今年16岁
child2.speak() // 我是子函数原型对象的方法
child2.say()   // error

```


- 特点：
1. 只继承了父类构造函数的属性，没有继承父类原型的属性。
2. 解决了原型链继承缺点 1、2、3。
3. 可以继承多个构造函数属性（call 多个）。
4. 在子实例中可向父实例传参。

- 缺点：
1. 只能继承父类构造函数的属性。
2. 无法实现构造函数的复用。（每次用每次都要重新调用）
3. 每个新实例都有父类构造函数的副本，臃肿。


3. **组合继承**（组合原型链继承和借用构造函数继承）（常用）

- 结合了上面两种模式的优点，传参和复用。

```js
function Child3(){
    Parent.call(this,'小明')
    this.childFn=function (){
        console.log('这是子的方法')
    }
}
Child3.prototype = new Parent()
Child3.prototype.speak = function(){console.log('我是子函数原型对象的方法')}

const child3 = new Child3()
child3.childFn()   // 这是子的方法
child3.parentFn()  // 我的名字是小明,我的性别是女,我今年16岁
child3.say()       // 我是构造函数原型对象的方法
child3.speak()     // 我是子函数原型对象的方法
```

- 特点： 
1. 可以继承父类原型上的属性，可以传参，可复用。 
2. 每个新实例引入的构造函数属性是私有的。

- 缺点：调用了两次父类构造函数（耗内存），子类的构造函数会代替原型上的那个父类构造函数。


4. **原型式继承**

- 用一个函数包装一个对象，然后返回这个函数的调用，这个函数就变成了个可以随意增添属性的实例或对象。object.create()就是这个原理。
- 类似于复制一个对象，用函数来包装。

```js
function content(obj){
    function F(){
      this.childFn = function (){ console.log('这个是子的方法')}
    }
    F.prototype = obj
    F.prototype.speak = function(){console.log('我是子函数原型对象的方法')}
    return new F();
}

const parent = new Parent()
const child4 = content(parent);


child4.childFn()   // 我是子的方法
child4.parentFn()  // 我的名字是小华,我的性别是女,我今年16岁
child4.say()       // 我是构造函数原型对象的方法
child4.speak()     // 我是子函数原型对象的方法
```


- 缺点：
- 1. 所有实例都会继承原型上的属性。 
- 2. 无法实现复用。（新实例属性都是后面添加的）

5. **寄生式继承**

- 就是给原型式继承外面套了个壳子。

```js
function content(obj){
    function F(){
      this.childFn = function (){ console.log('这个是子的方法')}
    }
    F.prototype = obj
    F.prototype.speak = function(){console.log('我是子函数原型对象的方法')}
    return new F();
}

const parent = new Parent()

function parentObject (obj){
    var sub = content(obj);
    sub.name = "小明";
    return sub;
}
const child5 = parentObject(parent);

child5.childFn()   // 我是子的方法
child5.parentFn() // 我的名字是小明,我的性别是女,我今年16岁
child5.say()      // 我是构造函数原型对象的方法
child5.speak()    // 我是子函数原型对象的方法
```

- 优点：没有创建自定义类型，因为只是套了个壳子返回对象（这个），这个函数顺理成章就成了创建的新对象。
- 缺点：没用到原型，无法复用。


6. **寄生组合式继承**（常用）

- 在函数内返回对象然后调用

```js
function content(obj){
    function F(){
      this.childFn = function (){ console.log('这个是子的方法')}
    }
    F.prototype = obj
    F.prototype.speak = function(){console.log('我是子函数原型对象的方法')}
    return new F();
}
const con = content(Parent.prototype)

function Sub(){
    Parent.call(this);
 }
Sub.prototype = con
const child6 = new Sub();

child6.childFn()   // 我是子的方法
child6.parentFn() // 我的名字是小华,我的性别是女,我今年16岁
child6.say()      // 我是构造函数原型对象的方法
child6.speak()    // 我是子函数原型对象的方法

```

- 优点：
1. 函数的原型等于另一个实例
2. 在函数中用 apply 或者 call 引入另一个构造函数，可传参
3. 修复了组合继承的问题
