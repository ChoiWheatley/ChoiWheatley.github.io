---
aliases: 
tags: 
description:
title: sockaddr(3type)
created: 2023-09-19T17:15:42
updated: 2023-09-19T17:16:41
---
<https://www.man7.org/linux/man-pages/man3/sockaddr.3type.html>
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
