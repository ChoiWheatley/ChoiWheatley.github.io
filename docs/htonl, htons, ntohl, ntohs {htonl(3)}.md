---
aliases: 
tags: 
description:
title: htonl, htons, ntohl, ntohs {htonl(3)}
created: 2023-09-19T17:23:29
updated: 2023-09-19T17:32:22
---
- <https://www.man7.org/linux/man-pages/man3/htonl.3.html>
- [[0017 C 🍎]]

```c
       #include <arpa/inet.h>

       uint32_t htonl(uint32_t hostlong);
       uint16_t htons(uint16_t hostshort);

       uint32_t ntohl(uint32_t netlong);
       uint16_t ntohs(uint16_t netshort);
```

## Description

호스트 주소를 네트워크 주소로 변환한다. 포트, IP주소 모두 지원하기 위해 long 버전과 short 버전이 각각 있다. 네트워크 바이트 오더는 언제나 빅 엔디언이다.
