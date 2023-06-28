---
title: JS数据类型
date: 2019-3-19
tags: JS
author: 映雪
---

花自飘零水自流，一种相思，两处闲愁。

<!--more-->

### 基本数据类型

- undefined、null、Boolean、String、Number、BigInt、Object、Symbol

### 数据类型分类

1. 简单数据类型

- undefined、null、Boolean、String、Number

2. 引用数据类型

- Function、 Array、 Object、 Date

- 简单数据类型: 简单数据类型就是保存在栈内存中的简单数据段，在内存中分别占有固定大小的空间，他们的值保存在栈空间中，我们通过按值访问。
- 引用数据类型: 值的大小不固定，栈内存中存放地址指向堆内存中的对象，是按引用访问的。栈内存中存放的只是该对象的访问地址。

### 数据类型

1. String 字符串

- 字符串可以是引号中的任意文本，可以使用单引号或双引号

```js
var name = '小华';
var name = '小华';
```

2. Number 数字

- 数字可以带小数点，可不带小数点

```js
var x = 34.0; //使用小数点来写
var x = 34; //不使用小数点来写
```

3. Boolean 布尔值

- 布尔值只有两个值 true 和 false

```js
var x = true;
var y = false;
```

4. Object 对象

- 对象由花括号分隔,在括号内部，对象的属性以名称和值对的形式 { name : value} 来定义,属性由逗号分隔

```js
var person = { name: '小华', id: 5566 };
```

5. null

- null 表示"没有对象"，即该处不应该有值

```js
var a = null;
console.log(a);
```

6. undefined

- undefined 表示"缺少值"，就是此处应该有一个值，但是还没有定义

```js
var i;
i; // undefined

function f(x) {
  console.log(x);
}
f(); // undefined

var o = new Object();
o.p; // undefined

var x = f();
x; // undefined
```

7. Symbol

- 凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。
 
```js
let s = Symbol();
 
typeof s  // symbol

```

### Nan

- 不是一个数字的数据值型数据，代表非法运算或者转换的结果 not a number 是一种特殊的 Number 类型，代表意外转换的数字，NaN 和任何东西都不相等。
- 自己都不等于自己

```js
var str = 'a2344';
console.log(typeof parseInt(str)); //结果 Nan
var str = '2344a';
console.log(typeof parseInt(str)); //结果 2344
console.log(Nan == Nan); //false
```

### 注意

- NaN 的数据类型是 number
- 数组(Array)的数据类型是 object
- 日期(Date)的数据类型为 object
- null 的数据类型是 object
- 未定义变量的数据类型为 undefined

### 类型转换

1. 强制类型转换

- 使用方法将数据类型转换为需要的数据类型。

```js
var a = 1;
var b = '1';
var c = true;

console.log(String(a));  // '1'
console.log(String(c));  // 'true'
console.log(Number(b));  // 1
console.log(Number(c));  // 1
console.log(Boolean(a)); // true
console.log(Boolean(b)); // true
```

2. 隐式类型转换

- 在运算过程中数据类型自动转换为可计算的数据类型。

```js
console.log(1 + true); //2
console.log(1 + false); //1
console.log(1 + undefined); //NaN
console.log(1 + NaN); //NaN
console.log(1 + null); //NaN
console.log(1 + { name: 'admin' }); //1[object object]
console.log(1 + [3, 4, 5]); //1 数组的值：13，4，5
if (1) {
  console.log('真');
} else {
  console('假');
} // 真
```

- if 隐式类型转换规则

1. 非 0 为 true，0 为 flase 发生了隐式类型转换成了布尔型 boolean
2. 非空字符为 true ，空字符为 false
3. undefined 和 Nav 和 null 为 false
4. 对象为真{}
5. 函数为真 function()
