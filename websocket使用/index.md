# WebSocket 使用


<!--more-->

> 🔍 WebSocket 的常用实践场景：[查看演示](http://websocket.demo.u9c8d.com/) | [演示代码](https://github.com/JiangBao/websocket-practice)

## 核心概念
[WebSocket](https://datatracker.ietf.org/doc/html/rfc6455) 是 HTML5 提供的一种浏览器与服务器间进行全双工通讯的技术，是基于 TCP 的应用层协议，客户端可以主动向服务器发送信息，服务器也可以主动向客户端推送信息。使用此 API，可以向服务器发送消息并接收事件驱动的响应，而不需要像短连接场景下通过轮询服务器获得响应数据。具体 API 可以参考 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)。

**连接过程：**
1. 通过 TCP 三次握手，建立整个通信的基础连接
2. TCP 连接成功后，客户端通过 HTTP 协议发送 WebSocket 版本号等信息，请求 upgrade
   ```
   GET / HTTP/1.1
   Upgrade: websocket
   Connection: Upgrade
   Host: localhost:3100
   Origin: http://localhost:3000
   Sec-WebSocket-Key: sN9cRrP/n9NdMgdcy2VJFQ==
   Sec-WebSocket-Version: 13
   ```
3. 服务端确认数据正确后，回复响应数据，返回 101 Switching Protocols
   ```
   HTTP/1.1 101 Switching Protocols
   Upgrade: websocket
   Connection: Upgrade
   Sec-WebSocket-Accept: fo7JIALyYQe5TVSylICkasKgHP4=
   Sec-WebSocket-Protocol: chat
   ```
4. 连接成功，维持心跳，通过 TCP 通道进行双工通信

**连接示例：**
```js
/**
 * ==================== web 客户端连接 ====================
 */
// 创建 websocket 连接
const socket = new WebSocket('ws://localhost:8080')

// 连接成功事件，监听到事件时向服务器发送数据 "PING!!!"
socket.addEventListener('open', (event) => {
  socket.send('PING!!!')
})

// 监听收到的数据
socket.addEventListener('message', (event) => {
  console.log('PONG: ', event.data)
})
```
```js
/**
 * ==================== 服务端，以 Node.js 为例 ====================
 * 使用 koa、koa-websocket
 */
const Koa = require('koa')
const router = require('koa-router')()
const websockify = require('koa-websocket')

const app = websockify(new Koa())

router.all('/ws', (ctx) => {
  ctx.websocket.send('Hello World')
  ctx.websocket.on('message', (msg) => {
    console.log('msg: ', msg)
  })
})

app.ws.use(router.routes())

app.listen(3100)
```

## 实践场景
以下实践代码均基于 web 平台，但这里的客户端不只限于浏览器，任何实现了 ws 协议的客户端都可以。  
*服务端 & 客户端代码仅为示例作用的最简实现方式，实际项目中还需考虑更严谨的实践*

1. 滚动日志/小红点：
   * [客户端代码](https://github.com/JiangBao/websocket-practice/blob/main/client/src/Carousel/index.js)
   * [服务端代码](https://github.com/JiangBao/websocket-practice/blob/main/server/app.js)，`demo === 'carousel'` 部分

2. 公共聊天室，类似 [gitter](https://gitter.im/)：  
   * [客户端代码](https://github.com/JiangBao/websocket-practice/blob/main/client/src/ChatRoom/index.js)
   * [服务端代码](https://github.com/JiangBao/websocket-practice/blob/main/server/app.js)，`demo === 'chat'` 部分
3. 在线游戏
   * [客户端代码](https://github.com/JiangBao/websocket-practice/blob/main/client/src/Game/index.js)
   * [服务端代码](https://github.com/JiangBao/websocket-practice/blob/main/server/app.js)，`demo === 'game'` 部分

