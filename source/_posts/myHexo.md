---
title: "webSocket协议"
date: 2019-02-21 15:52:49
tags:
---

> webSocket 是 HTNL5 新增的协议，她的目标是在浏览器和服务器之间建立一个不受限的双向通信的通道。

**webSocket 有以下特点：**

- 建立在 TCP 协议之上
- 与 HTTP 协议有良好的兼容性，默认端口也是 80 和 443，并且握手阶段采用 HTTP 协议
- 数据格式比较轻量，也可以发送二进制数据
- 没有同源限制，客户端可以与任意服务器通信
- 协议标识是`ws`,(如果加密，则为`wss`),服务器网址是 URL

> ws://example.com:80/some

```js
let ws = new WebSocket('wss://echo.websocket.org');
ws.onopen = function(e) {
  console.log('连接打开...');
  ws.send('hello webSocket');
};

ws.onmessage = function(e) {
  console.log(`接受到的信息${e.data}`);
  ws.close();
};

ws.onclose = function(e) {
  console.log('连接关闭');
};

ws.onerror = function(e) {
  console.log(e);
};
```

### webSocket.readyState

> `readyState` 属性返回实例对象的当前状态，共有 4 种状态

- CONNECTING:值为 0，表示正在连接
- OPEN:值为 1，表示连接成功，可以通信了
- CLOSEING:值为 2，表示连接正在关闭
- CLOSEED:值 3，表示连接已经关闭，或者打开连接失败

_WebSocket 是一种浏览器与服务器进行全双工通讯的网络技术，属于用于层协议。他基于 TCP 传输协议，并复用 HTTP 的握手通道_

WebSocket 为了保持客户端、服务端的实时双向通信，需要确保客户端、服务端之间的 TCP 通道保持连接没有断开。然而，对于长时间没有数据往来的连接，如果依旧长时间保持着，可能会浪费包括的连接资源。

但不排除有些场景，客户端、服务端虽然长时间没有数据往来，但仍需要保持连接。这个时候，可以采用心跳来实现。

发送方->接收方：ping
接收方->发送方：pong
ping、pong 的操作，对应的是 WebSocket 的两个控制帧，opcode 分别是 0x9、0xA。

> 举例，WebSocket 服务端向客户端发送 ping，只需要如下代码（采用 ws 模块）

```js
ws.ping('', false, true);

// 代码执行顺序
async function async1() {
  console.log('async1 start');
  await async2();
  console.log('async1 end');
}
async function async2() {
  console.log('async2');
}
console.log('script start');
setTimeout(function() {
  console.log('setTimeout');
}, 0);
async1();
new Promise(function(resolve) {
  console.log('promise1');
  resolve();
}).then(function() {
  console.log('promise2');
});
console.log('script end');

// script start
// async1 start
// async2
// promise1
// script end
// async1 end
// promise2
// setTimeout
```
