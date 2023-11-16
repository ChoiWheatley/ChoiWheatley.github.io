---
aliases: 
tags: 
description:
title: socket.io
created: 2023-11-16T08:35:45
updated: 2023-11-16T17:46:17
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

- **브로드캐스팅**은 서버가 연결된 클라이언트들에게 메시지를 보낼때 사용

```js
io.emit('hello'); // to all connected clients
socket.broadcast.emit('hello'); // 본인을 제외한 모두에게
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

## socket.io 품은 NestJS

[docs.nestjs.com/websockets](https://docs.nestjs.com/websockets/gateways)

`@WebSocketGetway(80, { namespace: 'events', transports: ['websocket'] })` 어노테이션을 가지는 게이트웨이 클래스를 활용.

[세 개의 인터페이스](https://docs.nestjs.com/websockets/gateways#lifecycle-hooks)를 사용하여 들어오는 connection 요청에 대한 루틴 실행

```ts
@WebSocketGetway(80, { namespace: 'events', transports: ['websocket'] })
class GameSessionGateway implements OnGatewayInit, OnGatewayConnection, OnGatewayDisconnect {
	private afterInit() {}
	private handleConnection() {}
	private handleDisconnect() {}
}
```

서버 인스턴스를 주입받을 수 있다. socket.io 에서의 io와 동일한 타입의 객체.

```js
	@WebSocketServer()
	io: Server;
```

HTTP때와 거의 비슷하게 파이프, 예외필터, 가드, 인터셉터 모두 달 수 있다.

```js
	@UsePipes(ValidationPipe)
	@SubscribeMessage('host')
	handleEvent(client: Client, data): WsResponse<unknown> {
		const event = 'events';
		return { event, data };
	}
```
