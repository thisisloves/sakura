---
title: Date对象
date: 2019-4-29
tags: JS
author: 映雪
---

短亭短，红尘辗，我把萧再叹。

<!--more-->

### Date 对象

- Date 对象用于处理日期与时间。

### Date 对象创建

```js
var d = new Date();
var d = new Date(milliseconds);
var d = new Date(dateString);
var d = new Date(year, month, day, hours, minutes, seconds, milliseconds);
```

### Date 对象方法

|         方法         |                           描述                           |
| :------------------: | :------------------------------------------------------: |
|      getDate()       |       从 Date 对象返回一个月中的某一天 (1 ~ 31)。        |
|       getDay()       |         从 Date 对象返回一周中的某一天 (0 ~ 6)。         |
|    getFullYear()     |             从 Date 对象以四位数字返回年份。             |
|      getHours()      |             返回 Date 对象的小时 (0 ~ 23)。              |
|  getMilliseconds()   |             返回 Date 对象的毫秒(0 ~ 999)。              |
|     getMinutes()     |             返回 Date 对象的分钟 (0 ~ 59)。              |
|      getMonth()      |             从 Date 对象返回月份 (0 ~ 11)。              |
|     getSeconds()     |             返回 Date 对象的秒数 (0 ~ 59)。              |
|      getTime()       |           返回 1970 年 1 月 1 日至今的毫秒数。           |
|     getUTCDate()     |     根据世界时从 Date 对象返回月中的一天 (1 ~ 31)。      |
|     getUTCDay()      |      根据世界时从 Date 对象返回周中的一天 (0 ~ 6)。      |
|   getUTCFullYear()   |         根据世界时从 Date 对象返回四位数的年份。         |
|    getUTCHours()     |        根据世界时返回 Date 对象的小时 (0 ~ 23)。         |
|   getUTCMinutes()    |        根据世界时返回 Date 对象的分钟 (0 ~ 59)。         |
|    getUTCMonth()     |        根据世界时从 Date 对象返回月份 (0 ~ 11)。         |
|   getUTCSeconds()    |        根据世界时返回 Date 对象的秒钟 (0 ~ 59)。         |
|      getYear()       |         已废弃。 请使用 getFullYear() 方法代替。         |
|       parse()        | 返回 1970 年 1 月 1 日午夜到指定日期（字符串）的毫秒数。 |
|      setDate()       |          设置 Date 对象中月的某一天 (1 ~ 31)。           |
|    setFullYear()     |           设置 Date 对象中的年份（四位数字）。           |
|      setHours()      |            设置 Date 对象中的小时 (0 ~ 23)。             |
|  setMilliseconds()   |            设置 Date 对象中的毫秒 (0 ~ 999)。            |
|     setMinutes()     |            设置 Date 对象中的分钟 (0 ~ 59)。             |
|      setMonth()      |             设置 Date 对象中月份 (0 ~ 11)。              |
|     setSeconds()     |            设置 Date 对象中的秒钟 (0 ~ 59)。             |
|      setTime()       |           setTime() 方法以毫秒设置 Date 对象。           |
|     setUTCDate()     |     根据世界时设置 Date 对象中月份的一天 (1 ~ 31)。      |
|      setYear()       |         已废弃。请使用 setFullYear() 方法代替。          |
|    toDateString()    |           把 Date 对象的日期部分转换为字符串。           |
|    toGMTString()     |         已废弃。请使用 toUTCString() 方法代替。          |
|    toISOString()     |           使用 ISO 标准返回字符串的日期格式。            |
|       toJSON()       |             以 JSON 数据格式返回日期字符串。             |
| toLocaleDateString() |  根据本地时间格式，把 Date 对象的日期部分转换为字符串。  |
| toLocaleTimeString() |  根据本地时间格式，把 Date 对象的时间部分转换为字符串。  |
|   toLocaleString()   |       根据本地时间格式，把 Date 对象转换为字符串。       |
|      toString()      |                把 Date 对象转换为字符串。                |
|    toTimeString()    |           把 Date 对象的时间部分转换为字符串。           |


```js
console.log(d.getFullYear());
console.log(d.getMonth());
console.log(d.getDate()); //获取日
console.log(d.getDay()); //获取星期配合swith
console.log(d.getHours());
console.log(d.getMinutes());
console.log(d.getSeconds());
console.log(d.getMillisecondes());
console.log(d.getTime()); //1970到现在的毫秒
```


### 计时器

- 按照指定的周期（以毫秒计）来调用函数或计算表达式。方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。

- setInterval(参数 1,参数 2);
- 参数 1：回调函数
- 参数 2：毫秒数
- 功能：每隔参数 2 的时间,执行一个参数 1 的函数

```js
var t=setInterval(function()){
console.log("hello")
},1000)

doucument.onclick = function () {
  clearInterval(t);
};
```

### 延时器

- 在指定的毫秒数后调用函数或计算表达式。

- setTimeout(参数 1,参数 2);
- 参数 1：回调函数
- 参数 2：毫秒数
- 功能：延迟参数 2 的时间，只执行一次参数 1 的函数

```js
var t = setInterval(function () {
  console.log('wrold');
}, 5000);

document.onclick = function () {
  clearTimeout(t);
};
```
