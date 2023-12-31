---
title: String对象
date: 2019-9-27
tags: JS
author: 映雪
---

春挽冬留独自悲，日月星辰几轮回，花间媚影谁留恋？待看红尘谁伴随？

<!--more-->

### String 对象

- String 对象用于处理文本（字符串）。

### String 对象创建

```js
var txt = new String('string');
// 或者
var a = 'string';
```

### String 对象方法

|        方法         |                                  描述                                   |
| :-----------------: | :---------------------------------------------------------------------: |
|      charAt()       |                         返回在指定位置的字符。                          |
|    charCodeAt()     |                 返回在指定的位置的字符的 Unicode 编码。                 |
|      concat()       |                连接两个或更多字符串，并返回新的字符串。                 |
|     endsWith()      |       判断当前字符串是否是以指定的子字符串结尾的（区分大小写）。        |
|   fromCharCode()    |                        将 Unicode 编码转为字符。                        |
|      indexOf()      |            返回某个指定的字符串值在字符串中首次出现的位置。             |
|     includes()      |                  查找字符串中是否包含指定的子字符串。                   |
|    lastIndexOf()    | 从后向前搜索字符串，并从起始位置（0）开始计算返回字符串最后出现的位置。 |
|       match()       |                  查找找到一个或多个正则表达式的匹配。                   |
|      repeat()       |              复制字符串指定次数，并将它们连接在一起返回。               |
|      replace()      |        在字符串中查找匹配的子串，并替换与正则表达式匹配的子串。         |
|    replaceAll()     |      在字符串中查找匹配的子串，并替换与正则表达式匹配的所有子串。       |
|      search()       |                      查找与正则表达式相匹配的值。                       |
|       slice()       |          提取字符串的片断，并在新的字符串中返回被提取的部分。           |
|       split()       |                       把字符串分割为字符串数组。                        |
|    startsWith()     |                  查看字符串是否以指定的子字符串开头。                   |
|      substr()       |                从起始索引号提取字符串中指定数目的字符。                 |
|     substring()     |                提取字符串中两个指定的索引号之间的字符。                 |
|    toLowerCase()    |                          把字符串转换为小写。                           |
|    toUpperCase()    |                          把字符串转换为大写。                           |
|       trim()        |                         去除字符串两边的空白。                          |
| toLocaleLowerCase() |               根据本地主机的语言环境把字符串转换为小写。                |
| toLocaleUpperCase() |               根据本地主机的语言环境把字符串转换为大写。                |
|      valueOf()      |                      返回某个字符串对象的原始值。                       |
|     toString()      |                            返回一个字符串。                             |
