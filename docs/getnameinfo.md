---
aliases: 
tags: 
description:
title: getnameinfo
created: 2023-09-18T14:33:17
updated: 2023-09-18T14:48:26
---
<https://www.man7.org/linux/man-pages/man3/getnameinfo.3.html>

> address-to-name translation in protocol independent  
> 프로토콜 독립적으로 서버 주소를 이름으로 변환해주는 녀석.

```c
#include <sys/socket.h>
#include <netdb.h>

int getnameinfo(const struct sockaddr *restrict addr, 
				socklen_t addrlen,
			    char host[_Nullable restrict .hostlen],
			    socklen_t hostlen,
			    char serv[_Nullable restrict .servlen],
			    socklen_t servlen,
			    int flags);
```

## description

`getnameinfo`는 `getaddrinfo`의 반대역할을 수행합니다. 이 함수는 소켓 주소를 프로토콜-독립적인 방법으로 각각 호스트, 서비스 주소로 변환합니다. 

`host`, `serv` 인자는 호출자가 할당하여야 하며, `getnameinfo` 호출시 주소 이름이 들어가게 됩니다.

## return value

마찬가지로, 리턴값을 에러처리용으로 씁니다.

0일땐 성공이며, node, service name이 인자에 채워집니다.

0 아닌 값이 들어오면 실패이며, 에러코드가 리턴됩니다. ~~어떨 땐 전역변수 `errno`가 채워지고 어떨땐 에러코드를 리턴하고.. 개판이네~~
