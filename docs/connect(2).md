---
aliases: 
tags: 
description:
title: connect(2)
created: 2023-09-19T17:04:11
updated: 2023-09-19T17:26:33
---
- <https://man7.org/linux/man-pages/man2/connect.2.html>
- [[0017 C 🍎]]
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

