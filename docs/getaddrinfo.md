---
aliases: 
tags: 
description:
title: getaddrinfo
created: 2023-09-18T14:32:25
updated: 2023-09-18T14:32:27
---
<https://man7.org/linux/man-pages/man3/getaddrinfo.3.html)>

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

hostname, host address, service name, port number를 한데모아 하나의 `struct addrinfo` 구조체에 담아주는 친절한 함수이다.
