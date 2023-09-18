---
aliases: 
tags: 
description:
title: mmap(2)
created: 2023-09-18T19:22:29
updated: 2023-09-18T20:55:57
---
- <https://www.man7.org/linux/man-pages/man2/mmap.2.html>
- <https://en.wikipedia.org/wiki/Mmap>
- [[9. Virtual Memory]]

```c
#include <sys/mman.h>
void *mmap(void addr[.length], size_t length, int prot, int flags,
  		   int fd, off_t offset);
int munmap(void addr[.length], size_t length);
```

## Description

`mmap`은 `fd`가 가리키는 파일의 `offset` 지점을 기준으로 `length`만큼을 가상 메모리 공간에 매핑해주는 함수이다.

- `addr`: 메모리 매핑 시작주소 **선호**위치를 커널에게 넘김. 즉, 커널이 다른 곳에 할당해 주어도 불만 가지면 안된다. Nullable
- `offset`: 반드시 `sysconf(_SC_PAGE_SIZE)`에 의해 명시된 페이지 사이즈의 배수여야 한다고
- `fd`: `mmap`함수 호출로 메모리가 매핑되면 매핑 영역을 훼손하지 않기 위해 바로 `fd`를 닫아야 한다.
- `prot`: memory **prot**ection의 약자. ~~별걸 다 줄이네..~~ 메모리 보호 규칙을 bitwise OR로 정의할 수 있다. 반드시 `fd` 열 때 먹였던 모드와 충돌하면 안된다.
	- 사용하는 메모리 보호규칙으로는 `PROT_EXEC`, `PROT_READ`, `PROT_WRITE`, `PROT_NONE`이 있다.
- `flag`: 매핑시 다른 프로세스들도 그 영역을 보거나 쓸 수 있는지(`MAP_SHARED`), Copy On Write를 사용할건지(`MAP_PRIVATE`) 결정한다.
	- `MAP_ANON[YMOUS]`: 얘는 좀 특이하다. `fd`값을 무시하고 어느 파일과도 매핑을 하지 않는다고 한다.
	- `MAP_FIXED`: 동시성 프로그래밍과 관련하여 주의해야 할 사항이 Notes에 적혀있다.

`munmap`은 매핑한 주소와 `length`사이만큼의 구간을 unmap 한다. 즉, 해당 구역의 메모리 공간을 비활성화 한다. `fd`를 닫는다고 unmap이 되지 않는다는 점 유의.

## Notes

`mmap`으로 매핑한 메모리 영역은 [[fork(2)]] 를 통해 공유될 수 있다.

## wiki

- `mmap`으로 할당받은 영역은 언제나 게으르게 파일을 읽거나 쓴다. 이것을 *Lazy demanding*이라고 부른다.
- `MAP_SHARED`로 매핑한 영역은 `fork`에 의한 영역을 보존하여 부모(또는 자식) 프로세스가 그 영역을 수정하면 다른 프로세스도 수정된 내용을 볼 수 있다.
- `MAP_PRIVATE`으로 매핑한 영역은 `fork`를 하게되면 Copy On Write 메커니즘으로 인해 프로세스마다 다른 값을 가지게 된다.
- [[msync(2)]] 시스템 콜을 호출하면 강제로 수정한 메모리 맵 영역을 파일로 밀어넣게 된다. `MAP_SHARED`로 매핑한 영역도 다른 프로세스들이 나의 수정사항을 보게 만들고 싶다면 수정 후`msync`를 호출해야 한다.

## Question

> 메모리 공간은 랜덤 액세스가 가능하기에 pointer arithmetic또한 가능합니다. `mmap`을 사용하여 파일과 메모리를 매핑하면 파일(디스크파일, 소켓)도 pointer arithmetic을 사용한 랜덤 액세스가 가능해지나요?

질문의 의도는 Unix IO만을 사용하면 순차읽기밖에 허용하지 않는데, `mmap`을 사용하면서 랜덤 액세스가 가능해지는 것이 아닌가 궁금했다. 디스크는 랜덤액세스가 가능한 반면, 소켓은 랜덤 액세스가 안되는 거 아닌가?

### Answer by @신병철

디스크도 sequential access이다. 다만 그 속도가 (상대적으로) 빨라서 랜덤 액세스 인것처럼 보이는 것이고. lazy demand한 속성으로 인해 메모리 맵 공간은 초기에는 아무것도 메모리에 올라와 있지 않다가 맵 공간 안 임의주소를 참조하면 시작지점부터 해당 주소(가 들어있는 페이지)까지를 읽어들인 뒤에 비로소 그 위치의 값을 참조하게 될 것이다.

stdin과 같이 스트림으로 들어오는 파일은 어떻게 될까? 실험을 해봐야 알겠지만 내 생각엔 해당 메모리 위치가 나올 때까지 Blocking 상태가 될 것 같다.

## 실험 with `mmap` & `stdin`

1. stdin을 `mmap`한다. 그 첫 주소의 위치를 `stdin_p`라고 하자.
2. `*(stdin_p + 10)`의 값을 출력한다. 그러면 분명 blocking 상태가 되겠지.
3. 의도적으로 터미널에 한 글자만(1 byte) 넣고 엔터를 쳐보자.
4. 긴 문자열 (적어도 10byte는 넘는)을 터미널에 넣고 엔터를 쳐보자.
