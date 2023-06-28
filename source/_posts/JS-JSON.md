---
title: JSON
date: 2019-10-5
tags: JS
author: 映雪
---

你一身薄凉模样，丢了衷肠，尘世里独自流浪，心事飞扬，无人懂你也曾悲欢失常，如今只剩下了这副皮囊，无言收场。

<!--more-->

### JSON

- JSON 是用于存储和传输数据的格式。
- JSON 通常用于服务端向网页传递数据 。

### JSON 语法规则

1. 数据为 键/值对
2. 数据由逗号分隔
3. 大括号保存对象
4. 方括号保存数组

### JSON 相关函数

|       函数       |                       描述                       |
| :--------------: | :----------------------------------------------: |
|   JSON.parse()   | 用于将一个 JSON 字符串转换为 JavaScript 对象。｜ |
| JSON.stringify() |    用于将 JavaScript 值转换为 JSON 字符串。｜    |

### JSON 与 JS 对象的关系

- JSON 是 JS 对象的字符串表示法。它使用文本表示一个 JS 对象的信息，（JSON）本质是一个字符串。

```js
var obj = { a: 'Hello', b: 'World' }; //这是一个js对象，注意js对象的键名也是可以使用引号包裹的,这里的键名就不用引号包含
var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
```

- JSON（格式字符串） 和 JS 对象（也可以叫 JSON 对象 或 JSON 格式的对象）互转（JSON.parse 和 JSON.stringify）
- 要实现从 JSON 字符串转换为 JS 对象，使用 JSON.parse() 方法：

```js
var obj = JSON.parse('{"a": "Hello", "b": "World"}'); //结果是 {a: 'Hello', b: 'World'}  一个对象
```
- 要实现从JS对象转换为JSON字符串，使用 JSON.stringify() 方法：

```js
var json = JSON.stringify({a: 'Hello', b: 'World'}); //结果是 '{"a": "Hello", "b": "World"}'  一个JSON格式的字符串
```

### JSON 去重

```js
unique = (arr, attribute) => {
  var new_arr = [];
  var json_arr = [];
  for (var i = 0; i < arr.length; i++) {
    if (new_arr.indexOf(arr[i][attribute]) === -1) {
      //  -1代表没有找到
      new_arr.push(arr[i][attribute]); //如果没有找到就把这个name放到arr里面，以便下次循环时用
      json_arr.push(arr[i]);
    } else {
    }
  }
  return json_arr;
};
```

- 使用

```js
lightManagerItem: [
  {
    device_Id: '1184770212306477058',
    device_Name: '温度巡检仪',
    height: '1',
    idkey: '60',
    lightid: '1193545143874011138',
    top: '0',
    type: '1',
    width: '0',
    x: '1',
    y: '2',
  },
  {
    device_Id: '1184770212306477059',
    device_Name: '温度巡检仪',
    height: '2',
    idkey: '40',
    lightid: '1193545143874011139',
    top: '1',
    type: '2',
    width: '3',
    x: '4',
    y: '5',
  },
  {
    device_Id: '1184770212306477060',
    device_Name: '温度巡检仪',
    height: '6',
    idkey: '60',
    lightid: '1193545143874011140',
    top: '4',
    type: '4',
    width: '4',
    x: '6',
    y: '7',
  },
];
this.unique(lightManagerItem, 'device_Name');
```

- 结果

```js
lightManagerItem: [
  {
    device_Id: '1184770212306477058',
    device_Name: '温度巡检仪',
    height: '1',
    idkey: '60',
    lightid: '1193545143874011138',
    top: '0',
    type: '1',
    width: '0',
    x: '1',
    y: '2',
  },
];
```
