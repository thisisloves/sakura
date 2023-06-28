---
title: jQuery选择器
date: 2018-4-29
tags: jQuery
author: 映雪
---

醉漾轻舟，信流引到花深处，尘缘相误，无计花间住。

<!--more-->

### jQuery 选择器

- jQuery 选择器允许您对 HTML 元素组或单个元素进行操作。
- jQuery 选择器基于元素的 id、类、类型、属性、属性值等"查找"（或选择）HTML 元素。 它基于已经存在的 CSS 选择器，除此之外，它还有一些自定义的选择器。
- jQuery 中所有选择器都以美元符号开头：$()。

### jQuery 初级选择器

|   选择器   | css 模式 | jQuery 模式 |                描述                 |
| :--------: | :------: | :---------: | :---------------------------------: |
|   标签名   |  div{}   |  $('div')   |    获取所有 div 标签的 DOM 元素     |
|     ID     |  box{}   |  $('#box')  |   获取一个 ID 为 box 的 DOM 对象    |
| Class 类名 |  .box{}  |  $('.box')  | 获取所有 class 名为 box 的 DOM 对象 |

- css3 的子选择器（不兼容 IE6），但是到了 jQuery 中，jQuery 会自行将不兼容 IE 的问题解决掉。

### jQuery 多级选择器

|   选择器   |   CSS 模式   |   jQuery 模式   |
| :--------: | :----------: | :-------------: |
| 群组选择器 | div,span,p{} | $('div,span,p') |
| 后代选择器 |  ul li a{}   |  $('ul li a')   |
| 通配选择器 |    \* {}     |     $('\*')     |

- 通配选择器：选择所有；对性能有极大的浪费所以不能在全局范围内使用，最好的方法就是在局部环境下使用；

### jQuery 高级选择器

(1) 层次选择器

|     选择器     | css 模式  | jQuery 模式  |              描述               |
| :------------: | :-------: | :----------: | :-----------------------------: |
|   后代选择器   | ul li a{} | $('ul li a') |      获取追溯到的所有元素       |
|    子选择器    |  div>p{}  |  $('div>p')  |         只获取子类节点          |
|  next 选择器   |  div+p{}  |  $('div+p')  | 只获取某节点后一个同级 DOM 元素 |
| nextAll 选择器 |  div~p{}  |  $('div~p')  |  获取某节点后所有同级 DOM 元素  |

- jQuery 为后代选择器提供了一个方法 find()这个 find 方法里面有一个参数，就是想要找到的后代（可以是标签，类名，ID）

```js
$('div p').css('color', 'red') == $('div').find('p').css('color', 'red');
```

- jQuery 为子选择器提供了一个方法，children()

```js
$('div>p').css('color', 'red') == $('div').children('p').css('color', 'red');
```

- jQuery 提供了 next(）, nextAll( ) 选择器，参数同上：注意 next（）选择器，只选择后一个元素。

```js
$('div+p').css('color', 'red') == $('div').next('p').css('color', 'red');
$('div~p').css('color', 'red') == $('div').nextAll('p').css('color', 'red');
```

- 注意：children() , next() , nextAll() 这些方法如果不传递参数的话， 那么就默认传递一个通配符\*，通常在使用这些选择器的时候，我们需要精准的选择元素，避免发生各种怪异结果。

(2). 属性选择器

|    CSS 模式     |     jQuery 模式      |                  描述                  |
| :-------------: | :------------------: | :------------------------------------: |
|   input[name]   |   $('input[name]')   |      获取具有这个属性的 DOM 元素       |
| input[name=XXX] | $('input[name=XXX]') | 获取具有属性且属性值为 XXX 的 DOM 元素 |
|  input[value]   |   $('input[value]    |     获取有 value  属性的 DOM 元素      |

(3). 伪类选择器

|   过滤器名    |    jQuery 语法    |            说明            |   返回   |
| :-----------: | :---------------: | :------------------------: | :------: |
|    :first     |   $('li:first')   |       选取第一个元素       | 单个元素 |
|     :last     |   $('li:last')    |      选取最后一个元素      | 单个元素 |
| ：not(选择器) | $('li:not(.red)') | 选取 class 不是 red 的元素 | 一组元素 |
|     :even     |   $('li:even')    |     选择偶数的所有元素     | 一组元素 |
|     :odd      |    $('li:odd')    |      选择所有奇数元素      | 一组元素 |
|     ：eq      |  $('li:eq（1）')  |     选择对应下标的元素     | 单个元素 |

(4). 内容过滤器

|    过滤器名     |       jQuery 语法        |                 说明                 |   返回   |
| :-------------: | :----------------------: | :----------------------------------: | :------: |
| :contains(text) | $('li:contains(123456)') |       选择有 123456 文本的元素       | 一组元素 |
|     :empty      |      $(li':empty')       | 选取 li 中不包含子元素或空文本的元素 | 一组元素 |
| ：has（选择器） |    $('ul:has(.red)')     |      选择子元素含有类 red 的 ul      | 一组元素 |

- jQuery 为了优化:has 选择器性能，提供了一个方法.has()

```js
$('ul:has(.red)') == $('ul').has('.red');
```

(5). 可见性选择器

| 过滤器名 |   jQuery 语法   |        说明        |   返回   |
| :------: | :-------------: | :----------------: | :------: |
| :hidden  |  $(li:hidden)   | 选取所有不可见元素 | 集合元素 |
| :visible | $('li:visible') |  选取所有可见元素  | 集合元素 |

- 注：是否可见的判定因素为 display：block & display :none

### jQueryDOM 对象

- jQueryDOM 对象和原生 JavaScriptDOM 对象之间的属性方法不通用

```js
$('DOM')[0].style.color = red;
$('DOM').get(0).style.color = red;
var obox = document.getElementById('box');
$(obox);
```
