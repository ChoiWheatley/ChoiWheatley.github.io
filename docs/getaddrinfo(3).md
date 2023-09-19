---
aliases: 
tags: 
description:
title: getaddrinfo(3)
created: 2023-09-18T14:32:25
updated: 2023-09-19T23:24:12
---
- <https://man7.org/linux/man-pages/man3/getaddrinfo.3.html>
- [[0017 C 🍎]]

```c
#include <sys/types.h>
#include <sys/socket.h>
#include <netdb.h>

int getaddrinfo(const char *restrict node,
			    const char *restrict service,
			    const struct addrinfo *restrict hints,
			    struct addrinfo **restrict res);

void freeaddrinfo(struct addrinfo *res);

const char *gai_strerror(int errcode);
```

```c
struct addrinfo {
	int              ai_flags;
	int              ai_family;
	int              ai_socktype;
	int              ai_protocol;
	socklen_t        ai_addrlen;
	struct sockaddr *ai_addr;
	char            *ai_canonname;
	struct addrinfo *ai_next;
};
```

hostname, host address, service name, port number를 한데모아 하나의 `struct addrinfo` 구조체에 담아주는 친절한 함수이다. 이중 포인터인 이유는 CSAPP Figure 11.15에서 볼 수 있다시피 연결리스트의 첫 주소를 가리키고 있기 때문이다. 연결리스트 head를 이중 포인터로 만든 덕분에 포인터의 값을 `getaddrinfo`가 스스로 할당해서 그 위치를 먹일 수 있게 되었다. 

`getaddrinfo`는 힙 메모리 영역 위에 할당받은 연결리스트를 사용하기 때문에 `freeaddrinfo` 함수를 호출하여 제거해 주어야 한다.

![[Pasted image 20230919190726.png]]
