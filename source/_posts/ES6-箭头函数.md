---
title: 箭头函数
date: 2018-5-12
tags: ES6
author: 映雪
---

一往情深深几许，深山夕照深秋风厢。

<!--more-->

### 箭头函数

- 将原函数的“function”关键字和函数名都删掉，并使用“=>”连接参数列表和函数体。

```js
function Fn(value) {
  return value;
}
const fn = (value) => {
  console.log(value);
  return value;
};
```

- 箭头函数相当于匿名函数，并且简化了函数定义。箭头函数有两种格式，一种只包含一个表达式，省略掉了{ ... }和 return。还有一种可以包含多条语句，这时候就不能省略{ ... }和 return。

```js
const a = (a, b) => a + b;
const b = (a) => {
  a = a + 1;
  return a;
};
```

### 箭头函数使用

1. 箭头函数没有自己的this对象，对于普通函数来说，内部的this指向函数运行时所在的对象，但是这一点对箭头函数不成立。它没有自己的this对象，内部的this就是定义时上层作用域中的this。也就是说，箭头函数内部的**this指向是固定**的，相比之下，普通函数的this指向是可变的。

```js
this.name = "小华"
this.age = 13
var people={
    name:'小明',
    age:12,
    function1:function(){
        return function(){
            console.log(this.name) // 小华
        }
    },
    function2:function(){
        return ()=>{
            console.log(this.name) // 小明
        }
    }
}
people.function1()()
people.function2()()
```

- this总是指向词法作用域，也就是外层调用者people
- this在箭头函数中已经按照词法作用域绑定了，所以，用call()或者apply()调用箭头函数时，无法对this进行绑定


2. 不可以当作构造函数，也就是说，不可以对箭头函数使用new命令，否则会抛出一个错误。

- new 步骤

(1). JS内部首先会先生成一个对象
(2). 再把函数中的this指向该对象
(3). 然后执行构造函数中的语句
(4). 最终返回该对象实例

```js
const Person = (e) => { this.name ="华"}
let a = new Person()
```

![截屏2022-02-23 上午11.43.01.png](/images/2022/02/23/Pdyz2VuHsjYNLpg.png)

- 箭头函数的this不可改,this指向永远不会随在哪里调用、被谁调用而改变，所以箭头函数不能作为构造函数使用。


3. 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

```js
function Fn(a){
    console.log(arguments) // Arguments(5) [1,2,3,4,4]
}
const fn = (...a)=>{
    console.log(a) // [1,2,3,4,4]
}
const Fun = (a)=>{
    console.log(arguments)  // arguments is not defined
}
Fn(1,2,3,4,4)
fn(1,2,3,4,4)
Fun(1,2,3,4,4)
```

4. 不可以使用yield命令，因此箭头函数不能用作 Generator 函数。

5. 箭头函数没有原型prototype

```js
let sayHi = () => { console.log('Hello World !') }; console.log(sayHi.prototype); // undefined 
```