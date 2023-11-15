---
aliases: 
tags: 
description:
title: 2023-11-15 {swjungle}{recre}
created: 2023-11-15T21:02:17
updated: 2023-11-15T21:57:07
---
- [[week14-18 {swjungle}{my own weapon}{nestjs, socketio}]]
___
- 게임 설정정보 요청 ⇛ 호스트가 원하는 게임을 선택했을때 새 게임 세션을 생성하여 `ws://` 프로토콜로 시작하는 URL을 반환한다. ⇛ 호스트 클라이언트는 해당 주소를 통해 게임 세션에 연결한다.

## socket.io 겉핥기

- sender

```js
socket.emit('hello', 'world', response => console.log(response)); // got it
```

- receiver

```js
socket.on('hello', (arg, callback) => {
	console.log(arg); // world
	callback('got it');
})
```

- 위처럼 일대일로 연결할 수도 있지만 **브로드캐스팅**도 수행할 수 있다고...!

```js
io.emit('hello'); // to all connected clients
io.to('news').emit('hello'); // to all connected clients in the 'news' room
```
