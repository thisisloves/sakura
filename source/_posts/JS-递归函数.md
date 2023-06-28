---
title: 递归函数
date: 2019-12-15
tags: JS
author: 映雪
---

千金纵买相如赋，脉脉此情谁诉。

<!--more-->

### 递归函数

- 编程语言中，函数直接或间接调用函数本身，则该函数称为递归函数。递归函数不能定义为内联函数。
- 递归函数将反复调用其自身，每调用一次就进入新的一层。所以递归函数必须有结束条件。
- 递归的时候，每次调用一个函数，计算机都会为这个函数分配新的空间，这就是说，当被调函数返回的时候，调用函数中的变量依然会保持原先的值，否则也不可能实现反向输出。

### 斐波那契数列

```
0, 1, 1, 2, 3, 5, 8, 13, 21...
```

- 个数列从第 3 项开始，每一项都等于前两项之和。

```js
const getList = (num) => {
  if (num == 1 || num == 2) {
    return num - 1;
  } else {
    return getList(num - 2) + getList(num - 1);
  }
};

const printList = (sum) => {
  let arr = [];
  for (let i = 0; i < sum; i++) {
    arr[i] = getList(i + 1);
  }
  return arr;
};

console.log(printList(12));
```

### 递归函数

```js
getId = (data, arr) => {
  data.map((item) => {
    if (item.children === undefined || item.children === null) {
      if (item.checked === true) {
        arr.push(item.code);
      }
    } else {
      if (item.children.length > 0) {
        if (item.checked === true) {
          arr.push(item.code);
        }
        this.getId(item.children, arr);
      }
    }
    return '';
  });
};
```

### 递归函数特点

1. 每一级函数调用时都有自己的变量，但是函数代码并不会得到复制，如计算 5 的阶乘时每递推一次变量都不同；
2. 每次调用都会有一次返回，如计算 5 的阶乘时每递推一次都返回进行下一次；
3. 递归函数中，位于递归调用前的语句和各级被调用函数具有相同的执行顺序；
4. 递归函数中，位于递归调用后的语句的执行顺序和各个被调用函数的顺序相反；
5. 递归函数中必须有终止语句。

### 递归函数缺点

1. 过深的调用会导致栈溢出
