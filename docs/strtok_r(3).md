---
aliases: 
tags: 
description:
title: strtok_r(3)
created: 2023-10-05T08:09:03
updated: 2023-10-05T08:14:34
---
- [[0017 C 🍎]]
- [한글 블로그](https://www.it-note.kr/86)
- [man page](https://man7.org/linux/man-pages/man3/strtok.3.html)
___

## synopsis

```c
       #include <string.h>

       char *strtok(char *restrict str, const char *restrict delim);
       char *strtok_r(char *restrict str, const char *restrict delim,
                      char **restrict saveptr);
```

`strtok_r`은 thread-safe한 버전의 `strtok`이다. 전역변수를 사용하지 않고 `saveptr`이라는 외부변수를 사용하기 때문.
