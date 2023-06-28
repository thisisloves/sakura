---
title: JS遍历
date: 2019-5-19
tags: JS
author: 映雪
---

流水潺潺，素手纤纤，青丝束，惊鸿若舞，衣袂飘飘，所谓倾心相许。

<!--more-->


### 遍历

- 遍历是指沿着某条搜索路线，依次对数据结构中每个节点均做一次访问。

### map

- map方法返回一个由原数组中的每个元素调用一个指定方法后的返回值组成的新数组。

- 语法

```js
array.map(function(currentValue,index,arr),thisValue)

```

1. currentValue：【必填】数组中正在处理的当前元素。
2. index：【可选】数组中正在处理的当前元素的索引。
3. arr：【可选】方法被调用的数组。也就是当前元素属于的数组对象。
4. thisValue：【可选】执行回调函数时使用的this值。

```js
let arr1 = [1, 2, 3, 4];
let obj = {
    a: 1
};
var arr2 = arr1.map(function(item,index,arr){
console.log(this);    // {a:1}
return item/2;
},obj);

console.log(arr2,arr1); //[0.5,1,1.5,2]  [1,2,3,4]
```

- 注意

1. map（）方法不会对空数组进行检测
2. map方法不会改变原始数组


### forEach

- forEach()方法返回一个由原数组中的每个元素调用一个指定方法后的返回值组成的新数组。

- 语法

```js
array.map(function(currentValue,index,arr),thisValue)

```

1. currentValue：【必填】数组中正在处理的当前元素。
2. index：【可选】数组中正在处理的当前元素的索引。
3. arr：【可选】方法被调用的数组。也就是当前元素属于的数组对象。
4. thisValue：【可选】执行回调函数时使用的this值。

```js
let arr1 = [1, 2, 3, 4];
let obj = {
    a: 1
};
arr1.forEach(function (self, index, arr) {
    console.log(`当前元素为${self}索引为${index},属于数组${arr}`);
    //做个简单计算
    console.log(self + this.a);
}, obj)
```

![截屏2022-03-07 上午10.59.57.png](/images/2022/03/07/Q5HXRF9Zfroev8y.png)

- 注意

1. forEach不支持break
2. forEach中使用return无效

### filter

- filter方法是对原数组进行过滤筛选，产生一个新的数组对象。

- 语法

```js
array.map(function(currentValue,index,arr),thisValue)

```

1. currentValue：【必填】数组中正在处理的当前元素。
2. index：【可选】数组中正在处理的当前元素的索引。
3. arr：【可选】方法被调用的数组。也就是当前元素属于的数组对象。
4. thisValue：【可选】执行回调函数时使用的this值。
 

```js
let arr = [1,1, 2, 3, 4];
var r = arr.filter(function (element, index, self) {
        return self.indexOf(element) == index;
});
console.log(arr,r)  //  [1, 1, 2, 3, 4]  [1, 2, 3, 4]
```


- 注意

1. filter()用于对数组进行过滤。
2. 创建一个新数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。
3. filter()不会对空数组进行检测、不会改变原始数组。

