---
aliases: 
tags: 
description:
title: connect(2)
created: 2023-09-19T17:04:11
updated: 2023-09-19T17:05:42
---
- <https://man7.org/linux/man-pages/man2/connect.2.html>
___

```c
#include <sys/socket.h>
int connect(int sockfd, const struct sockaddr *addr,
  		    socklen_t addrlen);
```

> establish connection, return 0 on success, -1 on failure  
> 서버에게 커넥션을 요청하는 시스템콜. 소켓을 그대로 인자에 넣는거 유의

```c
struct sockaddr {
    sa_family_t sa_family;
    char        sa_data[14];
}
```
