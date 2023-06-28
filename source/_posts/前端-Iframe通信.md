---
title: Iframe通信
date: 2019-12-10
tags: 前端
author: 映雪
---

尊前拟把归期说，未语春容先惨咽。

<!--more-->

### iframe

- iframe 是 html 元素，用于在网页中内嵌另一个网页。

### iframe 通信

- 多个系统之间互嵌套时，系统间通信就显得十分重要。

1. **通过 messenger.js 通信**

- 在需要通信的父窗体、和子窗体的文档中，都需要引入 MessengerJS。
- 窗体和子窗体各自的文档（document）中，都需要自己的 Messenger 与其他文档通信，父窗体和子窗体的 window 对象都对应着有且仅有一个 Messenger 对象，该 Messenger 对象会负责当前 window 的所有通信任务。因此，每个 Messenger 对象都需要唯一的名字，这样它们之间才可以知道是在跟谁通信。另外，MessengerJS 方案推荐指定项目名称，（类似命名空间的作用），以增强代码健壮性与组件复用性，避免未来与其他项目冲突。(注意: 项目名称应使用字符串类型)。

- 初始化 Messenger 对象

```js
import Messenger from '@/utils/messenger.js';

// 父窗口中 - 初始化Messenger对象
// 推荐指定项目名称, 避免Mashup类应用中, 多个开发商之间的冲突
var messenger = new Messenger('Parent', 'projectName');

// iframe中 - 初始化Messenger对象
// 注意! Messenger之间必须保持项目名称一致, 否则无法匹配通信
var messenger = new Messenger('iframe1', 'projectName');

// 多个iframe, 使用不同的名字
var messenger = new Messenger('iframe2', 'projectName');
```

- 在发现消息前，目标文档要确保已经监听了消息事件

```js
messenger.listen(function (msg) {
  alert('收到消息: ' + msg);
});
```

- 发消息时，要指定 messenger 的名字和消息，例如父窗体要给子窗体发消息

```js
// 父窗口中 - 向单个iframe发消息
messenger.targets['iframe1'].send(msg1);
messenger.targets['iframe2'].send(msg2);
// 父窗口中 - 向所有目标iframe广播消息
messenger.send(msg);
```

2. **通过 window.postmessage 通信**

- window.postMessage 方法可以安全地实现跨源通信，写明目标窗口的协议、主机地址或端口就可以发信息给它。这种是最简单的方式。

```js
// A 页面
window.addEventListener('message', function (event) {
  if (event.origin !== 'http://b.demo.com') return;
  toggleFullScreen();
});

// B 页面
parent.postMessage(value, 'http://a.demo.com');
```

3. **通过 domain 通信**

- document.domain 作用是获取/设置当前文档的原始域部分，同源策略会判断两个文档的原始域是否相同来判断是否跨域。这意味着只要把这个值设置成一样就可以解决跨域问题了。

- 将 domain 设置为一级域名的值，A 页面 url 为 a.xxx.com，A 页面中 iframe 引用的 B 页面 url 为 b.xxx.com，具体设置为
  document.domain = 'xxx.com'

- 设置完之后，在 a 页面的 window 上挂载使 iframe 全屏的方法

```js
// A页面
window.toggleFullScreen = () => {
  // do something
};
```

- 在 B 页面上可以直接获取到 A 页面的 window 对象并直接调用

```js
// b页面
window.parent.toggleFullScreen();
```

- 但是这个值的设置也有一定限制，只能设置为当前文档的上一级域或者是跟该文档的 URL 的 domain 一致的值。如 url 为 a.demo.com，那 domain 就只能设置为 demo.com 或者 a.demo.com。因此，设置 domain 的方法只能用于解决主域相同而子域不同的情况。所以这种情况下在使用上还是有限制的。


### iframe 的优点

- iframe 能够原封不动的把嵌入的网页展现出来。
- 如果有多个网页引用 iframe，那么你只需要修改 iframe 的内容，就可以实现调用的每一个页面内容的更改，方便快捷。
- 网页如果为了统一风格，头部和版本都是一样的，就可以写成一个页面，用 iframe 来嵌套，可以增加代码的可重用。
- 如果遇到加载缓慢的第三方内容如图标和广告，这些问题可以由 iframe 来解决。
