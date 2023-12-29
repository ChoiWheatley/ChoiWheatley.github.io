---
aliases: 
tags: 
description:
title: read(2)
created: 2023-09-18T14:02:42
updated: 2023-09-18T14:10:54
---
[read(2)](https://man7.org/linux/man-pages/man2/read.2.html)

```c
#include <unistd.h>
ssize_t read(int fd, void buf[.count], size_t count);
```

## description

`fd` 파일의 `count`바이트만큼을 읽어서 `buf`로 시작하는 문자열부터 담아주세요.

## return value

성공시 읽은 바이트의 개수를 리턴합니다. EOF를 만날시 0을 리턴합니다. 리턴한 값만큼 file position을 증가시켜놓습니다. (file desciptor 안에 file position이 있나?)

`count`보다 작은 값이 리턴되면 읽다가 중간에 EOF를 만난 것일 수도 있고, 파이프를 통해 읽고 있었더라면 시그널에 의해 interrupt된 걸 수도 있습니다.

-1을 리턴하면 `errno`가 설정된겁니다.

- `EAGAIN`: `fd`를 요청할 때 `O_NONBLOCK` 옵션을 마크하고, 읽기가 block상태가 된 경우입니다.
- `EBADF`: `fd`가 이상해요
- `EFAULT`: `buf`가 읽을 수 있는 영역이 아니예요
- **`EINTR`**: 데이터를 읽는 도중에 interrupt 시그널이 온 경우입니다.
- `EINVAL` 읽기가 불가능한 파일이예요. 사이즈 버퍼를 잘못 준 경우일 수도 있어요. fd에 타이머를 걸었을 때 타이머가 끝난 경우에도 set 됩니다.
- `EIO` IO 에러
- `EISDIR` 디렉토리를 읽으려고 했어요
