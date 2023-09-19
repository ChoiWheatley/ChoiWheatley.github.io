---
aliases: 
tags: 
description:
title: sockaddr(3type)
created: 2023-09-19T17:15:42
updated: 2023-09-19T17:30:41
---
- <https://www.man7.org/linux/man-pages/man3/sockaddr.3type.html>
- [[0017 C 🍎]]
___

```c
       #include <sys/socket.h>

       struct sockaddr {
           sa_family_t     sa_family;      /* Address family */
           char            sa_data[];      /* Socket address */
       };

       struct sockaddr_storage {
           sa_family_t     ss_family;      /* Address family */
       };

       typedef /* ... */ socklen_t;
       typedef /* ... */ sa_family_t;

//Internet domain sockets
       #include <netinet/in.h>

       struct sockaddr_in {
           sa_family_t     sin_family;     /* AF_INET */
           in_port_t       sin_port;       /* Port number */
           struct in_addr  sin_addr;       /* IPv4 address */
       };

       struct sockaddr_in6 {
           sa_family_t     sin6_family;    /* AF_INET6 */
           in_port_t       sin6_port;      /* Port number */
           uint32_t        sin6_flowinfo;  /* IPv6 flow info */
           struct in6_addr sin6_addr;      /* IPv6 address */
           uint32_t        sin6_scope_id;  /* Set of interfaces for a scope */
       };

       struct in_addr {
           in_addr_t s_addr;
       };

       struct in6_addr {
           uint8_t   s6_addr[16];
       };

       typedef uint32_t in_addr_t;
       typedef uint16_t in_port_t;

//UNIX domain sockets
       #include <sys/un.h>

       struct sockaddr_un {
           sa_family_t     sun_family;     /* Address family */
           char            sun_path[];     /* Socket pathname */
       };
```

## Description

- `sockaddr`: 소켓 주소를 정의하는 **제네릭** 타입
- `sa_family_t`: socket address family, 즉 어떤 구체적인 소켓 프로토콜을 의미하는지를 정의한다. 대표적으로 `AF_INET`, `AF_INET6`가 있고, 각각 IPv4, IPv6를 의미한다.
- `sockaddr_in`: IPv4 인터넷 도메인 소켓 주소를 정의하는 **구체**타입
	- `struct in_addr`: 네트워크 바이트 오더(빅 엔디언)으로 저장된 IP주소를 담는 구조체이다. 32비트 정수 멤버 하나 (`in_addr_t`) 를 갖고있다.
	- `in_port_t`: 네트워크 바이트 오더(빅 엔디언)로 변환된 `uint16_t` 타입 정수
- `sockaddr_in6`: IPv6 인터넷 도메인 소켓 주소를 정의하는 **구체**타입
- `sockaddr_un`: UNIX 도메인 소켓 주소를 정의하는 **구체**타입

모든 주소값(IP주소, Port)은 네트워크 바이트 오더로 구성되어 있으며, [[htonl, htons, ntohl, ntohs {htonl(3)}]]로 호스트 주소와 네트워크 주소를 변환할 수 있다.
