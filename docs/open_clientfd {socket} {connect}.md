---
aliases: 
tags: 
description:
title: open_clientfd {socket} {connect}
created: 2023-09-21T00:56:13
updated: 2023-09-21T01:01:42
---
- [[0017 C 🍎]]
- [[open_listenfd {socket}{bind}{listen}]]
- [[Socket Programming C API]]
- [[11. Network Programming {CSAPP}]]
___

## description

클라이언트가 서버에게 메시지를 전달하기 위해 필요한 과정 중 **socket**, **connect** 부분을 래핑했다. [[getaddrinfo(3)]]의 hint 인자에 어떤 플래그를 다는지 위주로 읽어보자.

```c
/*
 * open_clientfd - Open connection to server at <hostname, port> and
 *     return a socket descriptor ready for reading and writing. This
 *     function is reentrant and protocol-independent.
 *
 *     On error, returns:
 *       -2 for getaddrinfo error
 *       -1 with errno set for other errors.
 */
int open_clientfd(char *hostname, char *port) {
  int clientfd, rc;
  struct addrinfo hints, *listp, *p;

  /* Get a list of potential server addresses */
  memset(&hints, 0, sizeof(struct addrinfo));
  hints.ai_socktype = SOCK_STREAM; /* Open a connection */
  hints.ai_flags = AI_NUMERICSERV; /* ... using a numeric port arg. */
  hints.ai_flags |= AI_ADDRCONFIG; /* Recommended for connections */
  if ((rc = getaddrinfo(hostname, port, &hints, &listp)) != 0) {
    fprintf(stderr, "getaddrinfo failed (%s:%s): %s\n", hostname, port,
            gai_strerror(rc));
    return -2;
  }

  /* Walk the list for one that we can successfully connect to */
  for (p = listp; p; p = p->ai_next) {
    /* Create a socket descriptor */
    if ((clientfd = socket(p->ai_family, p->ai_socktype, p->ai_protocol)) < 0)
      continue; /* Socket failed, try the next */

    /* Connect to the server */
    if (connect(clientfd, p->ai_addr, p->ai_addrlen) != -1) break; /* Success */
    if (close(clientfd) < 0) {
      /* Connect failed, try another */  // line:netp:openclientfd:closefd
      fprintf(stderr, "open_clientfd: close failed: %s\n", strerror(errno));
      return -1;
    }
  }

  /* Clean up */
  freeaddrinfo(listp);
  if (!p) /* All connects failed */
    return -1;
  else /* The last connect succeeded */
    return clientfd;
}
```

**flags**

- `AI_NUMERICSERV`: `service` 인자에 들어갈 포트가 10진수 숫자임을 알려주는 플래그.
- `AI_ADDRCONFIG`: IPv4 addresses are returned in the list pointed to by `res` only if the local system has at least one IPv4 address configured, and IPv6 addresses are returned only if the local system has at least one IPv6 address configured.
	- 쉽게 말해 IPv4 서비스만 보고싶은 경우 필터링을 해준다고 한다.
