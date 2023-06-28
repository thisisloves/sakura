---
title: Proxy
date: 2020-1-29
tags: ES6
author: 映雪
---

静水流深，沧笙踏歌。

<!--more-->

### Proxy

- Proxy 对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）。
- Proxy 在目标对象的外层搭建了一层拦截，外界对目标对象的某些操作，必须通过这层拦截。

```
var proxy = new Proxy(target, handler);
```

- new Proxy()表示生成一个 Proxy 实例，target 参数表示所要拦截的目标对象，handler 参数也是一个对象，用来定制拦截行为。

### Proxy 实例方法

1. get()

- get 方法用于拦截某个属性的读取操作，可以接受三个参数，依次为目标对象、属性名和 proxy 实例本身（严格地说，是操作行为所针对的对象），其中最后一个参数可选。

```js
let student = {
  name: '小红',
  age: 15,
};
let proxy = new Proxy(student, {
  get(target, key) {
    let result = target[key];
    if (key === 'age') result += '岁';
    return result;
  },
});
console.log(student.age); // 15
console.log(proxy.age); // 15岁
```

- 通过 Proxy 拦截了 age 属性的读取。

2. set()

- set 方法用来拦截某个属性的赋值操作，可以接受四个参数，依次为目标对象、属性名、属性值和 Proxy 实例本身，其中最后一个参数可选。

```js
   let student = {
      name:'小红',
      age:15,
    }
    let proxy = new Proxy(student,{
      set(target,key,value){
      if(key === "age" && typeof value !== "number"){
        throw Error("age字段必须为number!")
      }
      // return Reflect.set(target,key,value);
        target[key] = value
        return target[key]
     }
     }
})
  console.log(student,proxy)
  student.age='1'
  proxy.age ='2' // 拦截赋值并抛出错误
  console.log(student,proxy)
```

3. has()

- has()方法用来拦截 HasProperty 操作，即判断对象是否具有某个属性时，这个方法会生效。典型的操作就是 in 运算符。

```js
   let student = {
      name:'小红',
      age:15,
    }
    let proxy = new Proxy(student,{
      has(target,key){
        if(key ==="name" || key==="age"){
          return target[key]
        }else{
          return false
        }
})
console.log('name' in proxy, 'name1' in proxy)  // true ,false
```

4. deleteProperty()

- deleteProperty()方法用于拦截 delete 操作，如果这个方法抛出错误或者返回 false，当前属性就无法被 delete 命令删除。

```js
let student = {
  name: '小红',
  age: 15,
};
let proxy = new Proxy(student, {
  deleteProperty(target, key) {
    if (key === 'age') {
      throw Error('无法删除了');
    } else {
      console.log('删除了');
      return true;
    }
  },
});
console.log(student, proxy);
delete student.name;
delete proxy.age; // 拦截赋值并抛出错误
console.log(student, proxy);
```

5. apply()

- apply方法拦截函数的调用、call和apply操作。
- apply方法可以接受三个参数，分别是目标对象、目标对象的上下文对象（this）和目标对象的参数数组。

```js
const Fn =(value) =>{
    return  value +  " Say:'I am Function'";
}
const pro = new Proxy(Fn,{
    apply(target,ctx,args){
    console.log(target,ctx,args)
    return args +  " Say:'I am Proxy'";
    }
})
console.log(Fn('Function'))
console.log(pro('Proxy'))// 拦截
```

6. construct()

- construct()方法用于拦截new命令。

```js
const handler = {
  construct (target, args, newTarget) {
    return new target(...args);
  }
};
```

- construct()方法可以接受三个参数。

1. target：目标对象。
2. args：构造函数的参数数组。
3. newTarget：创造实例对象时，new命令作用的构造函数（下面例子的p）。

```js
const p = new Proxy(function () {}, {
  construct: function(target, args) {
    console.log('called: ' + args.join(', '));
    return { value: args[0] * 10 };
  }
});

(new p(1)).value
// "called: 1"
// 10
```

- 注意:**construct()方法中的this指向的是handler,而不是实例对象。**


### Object.defineProperty()与 Proxy 区别

1. Object.defineProperty()只能监听某个属性不能全对象监听。
2. Proxy去代理，他会返回一个新的代理对象不会对原对象进行改动，而Object.defineProperty()是去修改原对象，修改原对象的属性，而proxy只是对原对象进行代理并给出一个新的代理对象
3. Proxy可以直接监听数组的变化。
4. Proxy有多达13种拦截方法,不限于apply、ownKeys、deleteProperty、has等等是Object.defineProperty不具备的。