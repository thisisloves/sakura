---
title: jQuery AJAX
date: 2018-8-26
tags: jQuery
author: 映雪
---

臂上妆犹在，襟间泪尚盈。水边灯火渐人行，天外一钩残月带三星。

<!--more-->

### AJAX

- 异步 JavaScript 和 XML（Asynchronous JavaScript and XML。
- AJAX 是与服务器交换数据的技术，它在不重载全部页面的情况下，实现了对部分网页的更新。

### AJAX load() 方法

- jQuery load() 方法是简单但强大的 AJAX 方法。
- load() 方法从服务器加载数据，并把返回的数据放入被选元素中。

```js
$(selector).load(URL, data, callback);
```

1. 参数 url， 可以提供筛选功能。

```js
$().load('data/test.html ');
$().load('data/text.html .box') >>> 筛选功能;
```

2. data 参数传入决定，是否以 post 方式提交参数是一个对象形式传入。

```js
$().load('data/test.html', { data: 'admin' });
```

3. 回调函数 function(response,status,xhr){}

```js
$().load('data/test.html', { data: 'admin' }, function (response, status, xhr) {});
```

(1). response ：返回结果，和页面里内容一样 。
(2). status ：状态，success，或者 error。
(3). xhr ：XMLHttpRequest，他是**一个对象**。

- xhr 有四个常用属性

|    属性名    |                                          说明                                          |
| :----------: | :------------------------------------------------------------------------------------: |
| responseText |                                作为响应主体被返回的文本                                |
| responseXML  | 如果响应主体内容类型是"text/xml"或"application/xml"， 则返回包含响应数据的 XMLDOM 文档 |
|    status    |                               响应的 HTTP 状态（状态码）                               |
|  statusText  |                                    HTTP 状态的说明                                     |

```html
<body>
  <div class="header"></div>
  <div class="cont"></div>
  <div class="footer"></div>
</body>
<script src="https://cdn.bootcss.com/jquery/2.2.4/jquery.js"></script>
<script>
  var url = 'http://localhost/day30/data/data.html';
  $('.cont').load(url, function () {
    //引用头部的部分填充进.cont
    $('cont')
      .find('ul')
      .click(function () {
        console.log(1);
      });
  });
  $('.footer').load('http://localhost/day30/data/footer.html p'); //引用页脚的部分的p填充.footer
</script>
```

### AJAX get() 方法

- jQuery get() 方法用于通过 HTTP GET 请求从服务器请求数据。GET 方法可能返回缓存数据。

```js
$.get(URL, callback);
```

- 例子

```js
$('button').click(function () {
  $.get('demo_test.php', function (data, status) {
    alert('数据: ' + data + '\n状态: ' + status);
  });
});
```

### AJAX post() 方法

- jQuery post() 方法通过 HTTP POST 请求向服务器提交数据。

```js
$.post(URL, data, callback);
```

- 例子

```js
$('button').click(function () {
  $.post(
    '/try/ajax/demo_test_post.php',
    {
      name: '11',
      url: 'http://www.baidu.com',
    },
    function (data, status) {
      alert('数据: \n' + data + '\n状态: ' + status);
    },
  );
});
```

### JQuery ajax() 方法

- ajax() 方法用于执行 AJAX（异步 HTTP）请求。所有的 jQuery AJAX 方法都使用 ajax() 方法。该方法通常用于其他方法不能完成的请求。
- jquery 封装 AJAX 的最顶层，用于特定情况下的使用。 有三个参数，与$.get()和$.post()方法，前三个参数相同。

```js
$('button').click(function () {
  $.ajax({
    url: 'demo_test.txt',
    success: function (result) {
      $('#div1').html(result);
    },
  });
});
```

- ajax 方法值

|            名称            |                                       值/描述                                        |
| :------------------------: | :----------------------------------------------------------------------------------: |
|           async            |                     布尔值，表示请求是否异步处理。默认是 true。                      |
|      beforeSend(xhr)       |                                发送请求前运行的函数。                                |
|           cache            |                 布尔值，表示浏览器是否缓存被请求页面。默认是 true。                  |
|    complete(xhr,status)    | 请求完成时运行的函数（在请求成功或失败之后均调用，即在 success 和 error 函数之后）。 |
|        contentType         |  发送数据到服务器时所使用的内容类型。默认是："application/x-www-form-urlencoded"。   |
|          context           |                      为所有 AJAX 相关的回调函数规定 "this" 值。                      |
|            data            |                              规定要发送到服务器的数据。                              |
|   dataFilter(data,type)    |                     用于处理 XMLHttpRequest 原始响应数据的函数。                     |
|          dataType          |                             预期的服务器响应的数据类型。                             |
|  error(xhr,status,error)   |                              如果请求失败要运行的函数。                              |
|           global           |           布尔值，规定是否为请求触发全局 AJAX 事件处理程序。默认是 true。            |
|         ifModified         |     布尔值，规定是否仅在最后一次请求以来响应发生改变时才请求成功。默认是 false。     |
|           jsonp            |                        在一个 jsonp 中重写回调函数的字符串。                         |
|       jsonpCallback        |                         在一个 jsonp 中规定回调函数的名称。                          |
|          password          |                        规定在 HTTP 访问认证请求中使用的密码。                        |
|        processData         |          布尔值，规定通过请求发送的数据是否转换为查询字符串。默认是 true。           |
|       scriptCharset        |                                  规定请求的字符集。                                  |
| success(result,status,xhr) |                               当请求成功时运行的函数。                               |
|          timeout           |                         设置本地的请求超时时间（以毫秒计）。                         |
|        traditional         |                      布尔值，规定是否使用参数序列化的传统样式。                      |
|            type            |                           规定请求的类型（GET 或 POST）。                            |
|            url             |                         规定发送请求的 URL。默认是当前页面。                         |
|          username          |                       规定在 HTTP 访问认证请求中使用的用户名。                       |
|            xhr             |                         用于创建 XMLHttpRequest 对象的函数。                         |
