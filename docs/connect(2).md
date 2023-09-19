---
aliases: 
tags: 
description:
title: connect(2)
created: 2023-09-19T17:04:11
updated: 2023-09-19T17:13:05
---
- <https://man7.org/linux/man-pages/man2/connect.2.html>
___

```c
#include <sys/socket.h>
int connect(int sockfd, const struct sockaddr *addr,
  		    socklen_t addrlen);
```

```c
struct sockaddr {
    sa_family_t sa_family;
    char        sa_data[14];
}
```

> establish connection, return 0 on success, -1 on failure  
> 서버에게 커넥션을 요청하는 시스템콜. 소켓을 그대로 인자에 넣는거 유의

`addr` 주소와 연결을 요청한 뒤 연결이 체결되면 `sockfd`로 스트림/데이터그램 접근이 가능하다.

[[sockaddr(3type)]]
`sockaddr` 타입은 제네릭 타입으로 각각 `sockaddr_in`과 `sockaddr_in6` 따위로 분리될 수 있다. 

- `struct in_addr`: 네트워크 바이트 오더(빅 엔디언)으로 저장된 IP주소를 담는 구조체이다. 32비트 정수 멤버 하나를 갖고있다.
- `struct sockaddr_in`: 소켓에 구체적인 주소를 지정한다. (IP, port)
	- `.sin_family`: Internet Family를 설정한다. IPv4인 경우 `AF_INET`, IPv6인 경우 `AP_INET6`이고 다른 옵션들도 많다.
	- `.sin_port`: 네트워크 바이트 오더(빅 엔디언)로 변환된 `uint16_t` 타입 정수
		- [`htons(uint16_t hostshort)`](https://www.man7.org/linux/man-pages/man3/htons.3.html): 포트번호를 변환한다.
	- `.sin_addr`: 클라이언트가 허용할 IP주소를 지정한다. `INADDR_ANY`는 `0.0.0.0`과 동일하다.