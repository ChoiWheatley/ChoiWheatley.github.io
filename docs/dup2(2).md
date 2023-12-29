---
aliases: 
tags: 
description:
title: dup2(2)
created: 2023-10-03T14:54:40
updated: 2023-10-03T14:57:11
---
- [[0017 C 🍎]]
- <https://www.man7.org/linux/man-pages/man2/dup.2.html>
___

## synopsis

```c
       #include <unistd.h>

       int dup(int oldfd);
       int dup2(int oldfd, int newfd);

       #define _GNU_SOURCE             /* See feature_test_macros(7) */
       #include <fcntl.h>              /* Definition of O_* constants */
       #include <unistd.h>

       int dup3(int oldfd, int newfd, int flags);
```

## gptbox summary

dup() 시스템 호출은 oldfd 디스크립터와 동일한 열린 파일 설명자를 가리키는 새로운 파일 디스크립터를 할당합니다. 새로운 파일 디스크립터 번호는 호출 프로세스에서 사용되지 않은 가장 낮은 번호의 파일 디스크립터임을 보장합니다. 성공적인 반환 후에는 이전 및 새 파일 디스크립터를 서로 교환하여 사용할 수 있습니다. 이 두 파일 디스크립터는 동일한 열린 파일 설명자를 참조하므로 파일 오프셋과 파일 상태 플래그를 공유합니다. 두 파일 디스크립터는 파일 디스크립터 플래그(FD_CLOEXEC)를 공유하지 않으며 dup()를 통해 생성된 복제 디스크립터의 close-on-exec 플래그는 꺼져 있습니다.

dup2() 시스템 호출은 dup()와 동일한 작업을 수행하지만 사용되지 않은 가장 낮은 번호의 파일 디스크립터 대신 newfd로 지정된 파일 디스크립터 번호를 사용합니다. 다시 말해, 파일 디스크립터 newfd는 이제 oldfd와 동일한 열린 파일 설명자를 가리키도록 조정됩니다. 파일 디스크립터 newfd가 이전에 열려 있었다면 재사용되기 전에 닫힙니다. 닫기는 무소음으로 수행되며(즉, dup2()에서 오류가 발생해도 보고되지 않음), 파일 디스크립터 newfd를 닫고 재사용하는 단계는 원자적으로 수행됩니다.

dup3()은 dup2()와 동일하지만 다음과 같은 차이점이 있습니다:
- 호출자는 플래그로 O_CLOEXEC을 지정하여 새 파일 디스크립터에 대한 close-on-exec 플래그를 설정할 수 있습니다.
- oldfd가 newfd와 같으면 dup3()는 EINVAL 오류와 함께 실패합니다.
