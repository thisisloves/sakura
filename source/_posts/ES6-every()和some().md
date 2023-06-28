---
title: every()和some()
date: 2020-2-12
tags: ES6
author: 映雪
---

绿蚁新醅酒,红泥小火炉。晚来天欲雪,能饮一杯无?

<!--more-->

### every()和some()

- every()是对数组中每一项运行给定函数，如果该函数对每一项返回true,则返回true。
- some()是对数组中每一项运行给定函数，如果该函数对任一项返回true，则返回true。

```js
var arr = [1,2,3,4,5,6];

console.log(arr.some(function(item,index,array){
    console.log('item='+item+ ',index='+index+',array='+array);
    return item > 3;
}));

console.log(arr.every(function(item,index,array){
    console.log('item='+item+',index='+index+',array='+array);
    return item > 3;
}))
```
- 结果

```
item = 1,index = 0 , array = 1,2,3,4,5,6
item = 2,index = 1 , array = 1,2,3,4,5,6
item = 3,index = 2 , array = 1,2,3,4,5,6
item = 4,index = 3 , array = 1,2,3,4,5,6
return true

item = 1,index = 0 , array = 1,2,3,4,5,6
return false 
```

- some 一直在找符合条件的值,一旦找到，就不会迭代下去
- every 从迭代开始，一旦有一个不符合条件,则不会继续迭代下去

- 注意
1.  every()  some() 不会对空数组进行检测。
2.  every()  some() 不会改变原始数组。

- 实例
- 判断一个数组是否包含另一个数组

```js
function isContainArr(parent, child) {
  return child.every(item => {
    return parent.some(v => {
      return item == v
    })
  })
}
let parent=[1,2,3,6,5,4]
let child=[1,3,4,6]
let child2=[1,3,4,6,7]
console.log(isContainArr(parent, child))//true
console.log(isContainArr(parent, child2))//false
```