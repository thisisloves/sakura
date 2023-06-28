---
title: WebSocket
date: 2017-10-30
tags: HTML
author: 映雪
---

眼有星辰大海，心有繁花似锦。

<!--more-->

![截屏2022-01-10 上午10.31.40.png](/images/2022/01/10/YwvsVOEfAGXRdSg.png)

- HTTP 协议有一个缺陷：通信只能由客户端发起。

### WebSocket

- 服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话,属于服务器推送技术的一种。

### WebSocket 特点

1. 建立在 TCP 协议之上，服务器端的实现比较容易。
2. 与 HTTP 协议有着良好的兼容性。默认端口也是 80 和 443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。
3. 数据格式比较轻量，性能开销小，通信高效。
4. 可以发送文本，也可以发送二进制数据。
5. 没有同源限制，客户端可以与任意服务器通信。
6. 协议标识符是 ws（如果加密，则为 wss），服务器网址就是 URL。

### WebSocket 用法

```js
var ws = null;

//判断当前浏览器是否支持WebSocket
if ('WebSocket' in window) {
  ws = new WebSocket('wss://echo.websocket.org');
} else {
  alert('当前浏览器不支持websocket');
}

ws.onopen = function (evt) {
  console.log('Connection open ...');
  ws.send('Hello WebSockets!');
};

ws.onmessage = function (evt) {
  console.log('Received Message: ' + evt.data);
  ws.close();
};

ws.onclose = function (evt) {
  console.log('Connection closed.');
};
```

### 客户端 API

1. WebSocket 构造函数

- WebSocket 对象作为一个构造函数，用于新建 WebSocket 实例。

```js
var ws = new WebSocket('ws://localhost:8080');
```

- 执行上面语句之后，客户端就会与服务器进行连接。

2. webSocket.readyState

- readyState 属性返回实例对象的当前状态，共有四种。

- CONNECTING：值为 0，表示正在连接。
- OPEN：值为 1，表示连接成功，可以通信了。
- CLOSING：值为 2，表示连接正在关闭。
- CLOSED：值为 3，表示连接已经关闭，或者打开连接失败。

```js
switch (ws.readyState) {
  case WebSocket.CONNECTING:
    // do something
    break;
  case WebSocket.OPEN:
    // do something
    break;
  case WebSocket.CLOSING:
    // do something
    break;
  case WebSocket.CLOSED:
    // do something
    break;
  default:
    // this never happens
    break;
}
```

3. webSocket.onopen

- 实例对象的 onopen 属性，用于指定连接成功后的回调函数。

```js
ws.onopen = function () {
  ws.send('Hello Server!');
};
```

- 如果要指定多个回调函数，可以使用 addEventListener 方法。

```js
ws.addEventListener('open', function (event) {
  ws.send('Hello Server!');
});
```

4. webSocket.onclose

- 实例对象的 onclose 属性，用于指定连接关闭后的回调函数。

```js
ws.onclose = function (event) {
  var code = event.code;
  var reason = event.reason;
  var wasClean = event.wasClean;
  // handle close event
};

ws.addEventListener('close', function (event) {
  var code = event.code;
  var reason = event.reason;
  var wasClean = event.wasClean;
  // handle close event
});
```

5. webSocket.onmessage

- 实例对象的 onmessage 属性，用于指定收到服务器数据后的回调函数。

```js
ws.onmessage = function (event) {
  var data = event.data;
  // 处理数据
};

ws.addEventListener('message', function (event) {
  var data = event.data;
  // 处理数据
});
```

6. webSocket.send()

- 实例对象的 send()方法用于向服务器发送数据。

```js
ws.send('your message');

// 发送 Blob 对象
var file = document.querySelector('input[type="file"]').files[0];
ws.send(file);

// 发送 ArrayBuffer 对象
var img = canvas_context.getImageData(0, 0, 400, 320);
var binary = new Uint8Array(img.data.length);
for (var i = 0; i < img.data.length; i++) {
  binary[i] = img.data[i];
}
ws.send(binary.buffer);
```

7. webSocket.bufferedAmount

- 实例对象的 bufferedAmount 属性，表示还有多少字节的二进制数据没有发送出去。它可以用来判断发送是否结束。

```js
var data = new ArrayBuffer(10000000);
socket.send(data);

if (socket.bufferedAmount === 0) {
  // 发送完毕
} else {
  // 发送还没结束
}
```

8. webSocket.onerror

- 实例对象的 onerror 属性，用于指定报错时的回调函数。

```js
socket.onerror = function (event) {
  // handle error event
};

socket.addEventListener('error', function (event) {
  // handle error event
});
```
