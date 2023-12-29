---
aliases: 
tags: 
description:
title: open_listenfd {socket}{bind}{listen}
created: 2023-09-21T00:57:58
updated: 2023-09-21T01:01:28
---
- [[0017 C 🍎]]
- [[open_clientfd {socket} {connect}]]
- [[Socket Programming C API]]
- [[11. Network Programming {CSAPP}]]
___
서버가 클라이언트의 요청을 받기 위한 소켓을 만드는 과정 중 **socket**, **bind**, **listen**을 추상화했다.

[[getaddrinfo(3)#gpt question `getaddrinfo(NULL, ...)` 하는 이유]] 의 대답과 같이 들어오는 모든 요청에 대하여 포트번호만 일치하면 listen하는 소켓을 만들어준다.

```c
/*
 * open_listenfd - Open and return a listening socket on port. This
 *     function is reentrant and protocol-independent.
 *
 *     On error, returns:
 *       -2 for getaddrinfo error
 *       -1 with errno set for other errors.
 */
/* $begin open_listenfd */
int open_listenfd(char *port) {
  struct addrinfo hints, *listp, *p;
  int listenfd, rc, optval = 1;

  /* Get a list of potential server addresses */
  memset(&hints, 0, sizeof(struct addrinfo));
  hints.ai_socktype = SOCK_STREAM;             /* Accept connections */
  hints.ai_flags = AI_PASSIVE | AI_ADDRCONFIG; /* ... on any IP address */
  hints.ai_flags |= AI_NUMERICSERV;            /* ... using port number */
  if ((rc = getaddrinfo(NULL, port, &hints, &listp)) != 0) {
    fprintf(stderr, "getaddrinfo failed (port %s): %s\n", port,
            gai_strerror(rc));
    return -2;
  }

  /* Walk the list for one that we can bind to */
  for (p = listp; p; p = p->ai_next) {
    /* Create a socket descriptor */
    if ((listenfd = socket(p->ai_family, p->ai_socktype, p->ai_protocol)) < 0)
      continue; /* Socket failed, try the next */

    /* Eliminates "Address already in use" error from bind */
    setsockopt(listenfd, SOL_SOCKET,
               SO_REUSEADDR,  // line:netp:csapp:setsockopt
               (const void *)&optval, sizeof(int));

    /* Bind the descriptor to the address */
    if (bind(listenfd, p->ai_addr, p->ai_addrlen) == 0) break; /* Success */
    if (close(listenfd) < 0) { /* Bind failed, try the next */
      fprintf(stderr, "open_listenfd close failed: %s\n", strerror(errno));
      return -1;
    }
  }

  /* Clean up */
  freeaddrinfo(listp);
  if (!p) /* No address worked */
    return -1;

  /* Make it a listening socket ready to accept connection requests */
  if (listen(listenfd, LISTENQ) < 0) {
    close(listenfd);
    return -1;
  }
  return listenfd;
}
```
