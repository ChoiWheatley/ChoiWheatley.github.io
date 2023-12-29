---
aliases: 
tags: 
description:
title: open(2)
created: 2023-09-20T11:17:10
updated: 2023-09-20T11:26:01
---
- <https://www.man7.org/linux/man-pages/man2/open.2.html>
- [[0017 C 🍎]]
___

```c
       #include <fcntl.h>

       int open(const char *pathname, int flags);
       int open(const char *pathname, int flags, mode_t mode);

       int creat(const char *pathname, mode_t mode);

       int openat(int dirfd, const char *pathname, int flags);
       int openat(int dirfd, const char *pathname, int flags, mode_t mode);

       /* Documented separately, in openat2(2): */
       int openat2(int dirfd, const char *pathname,
                   const struct open_how *how, size_t size);
```

## Description

**flags**

- access modes
	- `O_RDONLY`
	- `O_WRONLY`
	- `O_RDWR`
- file creation flags, file status flags
	- `O_CLOEXEC` enable close-on-exec flag
	- `O_CREAT` if pathname does not exist, create it as a regular file
	- `O_DIRECTORY` if pathname is not a directory, cause the open to fail
	- `O_EXCL`
	- `O_NOCTTY`
	- `O_NOFOLLOW`
	- `O_TMPFILE`
	- `O_APPEND` open in append mode
	- `O_ASYNC` enable signal-driven io
	- 

자주 쓰는 플래그: 여러 프로세스들 간에 독점적인 읽기를 보장해주는 옵션. 일종의 lock 파일처럼 쓸 수 있게된다.

```
O_CREAT | O_EXCL
```
