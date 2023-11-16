---
aliases: 
tags: 
description:
title: socket.io
created: 2023-11-16T08:35:45
updated: 2023-11-16T10:18:34
---
- [[week14-18 {swjungle}{my own weapon}{nestjs, socketio}]]
- [socket.io document](https://socket.io/docs/v4/)
___
<iframe src="https://socket.io/images/logo.svg" allow="fullscreen" allowfullscreen="" style="height: 20%; width: 20%; aspect-ratio: 1 / 1;"></iframe>

## CHEATSHEET

<https://socket.io/docs/v4/emit-cheatsheet>

- sender

```js
socket.emit('hello', 'world');
```

- receiver

```js
socket.on('hello', (arg) => console.log(arg)) // world
```

- 위처럼 일대일로 연결할 수도 있지만 **브로드캐스팅**도 수행할 수 있다고...!

```js
io.emit('hello'); // to all connected clients
io.to('news').emit('hello'); // to all connected clients in the 'news' room
```

- [client options](https://socket.io/docs/v4/client-options/)

소켓 체결시에 쿼리를 날릴 수도 있다. 클라이언트는 `io({query: {...}})` 스니펫을 사용하고 서버는 다음과 같이 사용할 수 있다.

```js
io.on('connection', socket => {
	console.log(socket.handshake.query);
});
```

- [`ack` argument 콜백함수](https://socket.io/docs/v4/client-api/#socketemiteventname-args)

request + response 쌍으로 주고받을 수도 있다.

```js
// client
socket.emit('hello', 'world', (res) => console.log(res)); // got it
```

```js
// server
io.on('connection', (socket) => {
	socket.on('hello', (arg, callback) => {
		console.log(arg); // world
		callback('got it');
	});
});
```

- room
	- 서버 ⟶ 클라 in room 메시지 보낼때: `io.to('my room').emit('hello')`
 
- namespace

```js
// server
io.of('/my-namespace').on('connection', (socket) => {
	socket.on('hello', (arg) => console.log(arg));
	// ...
});
```

- [express-session](https://socket.io/how-to/use-with-express-session)을 함께 쓰면 