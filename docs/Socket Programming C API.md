---
aliases: 
tags: 
description:
title: Socket Programming C API
created: 2023-09-16T16:57:23
updated: 2023-09-19T16:49:04
---
- [[0017 C 🍎]]
___

## Socket Programming Tutorial In C For Beginners

<iframe title="Socket Programming Tutorial In C For Beginners | Part 1 | Eduonix" src="https://www.youtube.com/embed/LtXEMwSG5-8?feature=oembed" height="113" width="200" allowfullscreen="" allow="fullscreen" style="aspect-ratio: 1.76991 / 1; width: 50%; height: 50%;"></iframe>

**Client socket workflow**

```mermaid
graph LR;
socket --> connect
connect --> id1["send or write"]
connect --> id2["recv or read"]
```

- [[socket(2)]] `socket(AF_INET, SOCK_STREAM, 0)`: 소켓을 생성하여 file descriptor를 리턴받는다.
- `struct in_addr`: 네트워크 바이트 오더(빅 엔디언)으로 저장된 IP주소를 담는 구조체이다. 32비트 정수 멤버 하나를 갖고있다.
- `struct sockaddr`: 소켓에 구체적인 주소를 지정한다. (IP, port)
	- `.sin_family`: Internet Family를 설정한다. IPv4인 경우 `AF_INET`, IPv6인 경우 `AP_INET6`이고 다른 옵션들도 많다.
	- `.sin_port`: 네트워크 바이트 오더(빅 엔디언)로 변환된 `uint16_t` 타입 정수
		- [`htons(uint16_t hostshort)`](https://www.man7.org/linux/man-pages/man3/htons.3.html): 포트번호를 변환한다.
	- `.sin_addr`: 클라이언트가 허용할 IP주소를 지정한다. `INADDR_ANY`는 `0.0.0.0`과 동일하다.
- [[connect(2)]]
- `recv(socket, char * server_response, size of buffer, 0)`: recieve data from the server
- `close(socket)`

**Server socket workflow**

```mermaid
graph LR;
socket --> bind
bind --> listen
listen --> accept
accept --> id1["send or write"]
accept --> id2["recv or read"]
```

- [[bind(2)]] :  소켓에 ip, port쌍을 바인드 한다.
- `listen(socket, backlog)`: `accept`를 통해 들어오는 연결들을 관리한다. `backlog`를 사용하여 최대 들어오는 요청 수를 정의할 수는 있는데, 동시접속을 허용하려면 동시성 프로그래밍을 다뤄야 한다...
- `accept(socket, struct sockaddr *, socklen_t *)`: 클라이언트 소켓을 받는다. 두 번째, 세 번째 인자를 넣어 클라이언트의 주소를 얻을 수 있다.
- `send(socket, char *buf, int buflen, flag)`: 지정한 소켓으로 메시지를 보낸다.
