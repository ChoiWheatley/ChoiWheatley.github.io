---
aliases: 
tags: 
description:
title: getaddrinfo(3)
created: 2023-09-18T14:32:25
updated: 2023-09-21T00:30:13
---
- <https://man7.org/linux/man-pages/man3/getaddrinfo.3.html>
- [[0017 C 🍎]]
- [[11. Network Programming {CSAPP}#11.4.7. Host and Service Conversion]] 에서 건너옴

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

## gpt question: `getaddrinfo(NULL, ...)` 하는 이유

**Question**

> 다음 함수는 getaddrinfo를 사용하여 struct addrinfo 리스트를 얻어내 순회를 돌면서 socket, bind 함수를 호출해. 여기서 궁금한 점은 왜 getaddrinfo 첫번째 인자를 NULL로 주는거야? ip주소가 들어가는 자리인 걸로 아는데 NULL이 들어가면 무엇으로 대체되는지 궁금해.

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
int open_listenfd(char *port) 
{
    struct addrinfo hints, *listp, *p;
    int listenfd, rc, optval=1;

    /* Get a list of potential server addresses */
    memset(&hints, 0, sizeof(struct addrinfo));
    hints.ai_socktype = SOCK_STREAM;             /* Accept connections */
    hints.ai_flags = AI_PASSIVE | AI_ADDRCONFIG; /* ... on any IP address */
    hints.ai_flags |= AI_NUMERICSERV;            /* ... using port number */
    if ((rc = getaddrinfo(NULL, port, &hints, &listp)) != 0) {
        fprintf(stderr, "getaddrinfo failed (port %s): %s\n", port, gai_strerror(rc));
        return -2;
    }

    /* Walk the list for one that we can bind to */
    for (p = listp; p; p = p->ai_next) {
        /* Create a socket descriptor */
        if ((listenfd = socket(p->ai_family, p->ai_socktype, p->ai_protocol)) < 0) 
            continue;  /* Socket failed, try the next */

        /* Eliminates "Address already in use" error from bind */
        setsockopt(listenfd, SOL_SOCKET, SO_REUSEADDR,    //line:netp:csapp:setsockopt
                   (const void *)&optval , sizeof(int));

        /* Bind the descriptor to the address */
        if (bind(listenfd, p->ai_addr, p->ai_addrlen) == 0)
            break; /* Success */
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

**answer**

`getaddrinfo` 함수에서 첫 번째 인자를 `NULL`로 전달하는 이유는 서버 소켓을 생성하고, 해당 소켓을 모든 사용 가능한 네트워크 인터페이스(로컬 주소)에 바인딩하기를 원하기 때문입니다.

`getaddrinfo` 함수의 첫 번째 인자는 호스트 이름이나 IP 주소를 나타냅니다. 만약 이 인자에 `NULL`을 전달하면, `getaddrinfo`는 현재 호스트(로컬 시스템)에 대한 네트워크 인터페이스 주소를 가져오게 됩니다. 이는 서버가 현재 호스트의 모든 네트워크 인터페이스(예: 모든 IP 주소)에서 들어오는 연결을 수락하고자 할 때 유용합니다. 즉, 서버는 모든 네트워크 인터페이스에 대한 연결을 수신하려고 할 때 `NULL`을 사용하여 `getaddrinfo`를 호출할 수 있습니다.

따라서 `getaddrinfo(NULL, port, &hints, &listp)`는 현재 호스트의 모든 네트워크 인터페이스에서 `port` 번호로 들어오는 연결을 수락하기 위한 주소 정보를 얻는 데 사용됩니다. 이렇게 얻은 주소 정보를 이용하여 서버 소켓을 생성하고 해당 주소로 바인딩합니다. 결과적으로 서버는 모든 네트워크 인터페이스에서 들어오는 연결을 수신할 수 있게 됩니다.

**정리**

bind를 할 때 넣는 `struct addrinfo` 구조체 안에 들어가는 주소와 포트에 대하여 주소를 NULL로 주면 알아서 "해당 포트로 들어오는 모든 호스트로부터의 접속을 허용합니다"라고 뜻하는 것 같다.

## gpt question `getaddrinfo` hint 넣는법

**question**

> 서버에게 HTTP GET 요청을 전송하려고 하는데 getaddrinfo 함수 인자를 어떻게 넣어야 올바른 struct addrinfo 리스트를 얻을 수 있을까?

**answer**

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <netdb.h>

int main() {
    struct addrinfo hints, *res;
    int status;

    // 서버 호스트 이름 또는 IP 주소, 포트 번호, 힌트 초기화
    const char *hostname = "www.example.com"; // 대상 서버 주소
    const char *port = "80"; // HTTP 포트 번호
    memset(&hints, 0, sizeof hints);
    hints.ai_family = AF_UNSPEC; // IPv4 또는 IPv6
    hints.ai_socktype = SOCK_STREAM; // TCP 소켓

    // getaddrinfo 함수를 사용하여 서버 주소 정보 얻기
    if ((status = getaddrinfo(hostname, port, &hints, &res)) != 0) {
        fprintf(stderr, "getaddrinfo error: %s\n", gai_strerror(status));
        return 1;
    }

    // addrinfo 리스트(res)를 사용하여 서버에 HTTP GET 요청을 전송할 수 있음

    // addrinfo 리스트를 사용한 후 메모리 해제
    freeaddrinfo(res);

    return 0;
}

```

위 예제에서 주요한 부분은 다음과 같습니다:

1. `getaddrinfo` 함수를 호출할 때 `hostname` 변수에 대상 서버의 호스트 이름 또는 IP 주소를 지정하고, `port` 변수에는 HTTP 포트 번호(일반적으로 80)를 지정합니다.
2. `hints` 구조체를 초기화하고, 필요한 네트워크 속성을 설정합니다. 위의 예제에서는 IPv4 또는 IPv6 주소를 사용하고, TCP 소켓을 지정했습니다.
3. `getaddrinfo` 함수를 호출하여 `res` 포인터를 통해 주소 정보를 얻습니다. 이렇게 얻은 `struct addrinfo` 리스트(`res`)에는 서버의 주소 정보가 들어 있습니다.
4. `addrinfo` 리스트를 사용하여 HTTP GET 요청을 서버에 전송할 수 있습니다. 이 부분은 HTTP 라이브러리나 소켓 프로그래밍을 통해 수행할 수 있습니다.
5. 작업이 끝난 후, 메모리 누수를 방지하기 위해 `freeaddrinfo` 함수를 사용하여 `res`에 할당된 메모리를 해제합니다.

이렇게 설정된 `getaddrinfo` 함수 호출을 통해 올바른 서버 주소 정보를 얻고, 이를 사용하여 HTTP GET 요청을 전송할 수 있게 됩니다.
