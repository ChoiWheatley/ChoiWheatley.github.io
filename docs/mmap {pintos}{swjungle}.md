---
aliases: 
tags: 
description:
title: mmap {pintos}{swjungle}
created: 2023-10-17T22:45:32
updated: 2023-10-17T23:23:10
---
- [[week07-09 {swjungle} {pintos}]]
- [[week09 - Virtual Memory {pintos} {swjungle}]]
- [[mmap(2)]]
- <https://casys-kaist.github.io/pintos-kaist/project3/memory_mapped_files.html>
___

## 요약

file-mapped virtual memory. 파일을 읽을 때 뒤늦게 가상주소와 파일 페이지가 담긴 커널가상주소 매핑을 시도하라. 파일을 수정할 때 커널 메모리에 수정만 해놓고 바로 파일에 수정사항을 보내지 말라. unmap, swapped out 상황에 dirty한 경우에만 내려보내자.

```c
void *mmap (void *addr, size_t length, int writable, int fd, off_t offset);
```

- 실패하는 케이스:
	- 인자에 `addr`이 PG_SIZE로 나누어 떨어지지 않는 경우
	- overlap with other reserved pages
- length가 딱 떨어지지 않으면 페이지 로드할 때 비는 공간을 0으로 채워넣되, **write back** 시에 해당 부분을 제외하고 내려보내야 한다.

유저는 malloc을 사용할 수 없지만 `addr`을 채워넣어야 하기 때문에 유저 가상주소를 직접 작성하여 넣어주어야 한다(...) 리눅스처럼 똑똑하게 NULL이 들어가면 알아서 메모리 할당해주지 않는다는 점 알아둘 것.

```c
void munmap (void *addr);
```

- implicit unmap
	- process exit
- `munmap` doesn't actually unmaps space, until implicit/explicit unmapping
- `file_reopen`: mapping과 별개의 파일을 열기위해 사용.
- 두 프로세스가 같은 파일을 매핑하는데 동일한 프레임 (inode?)을 참조함.

> Closing or removing a file does not unmap any of its mappings  
> 파일을 닫거나 지우는 행위가 unmap을 하지 않습니다.

fd가 닫혀있어도 `munmap`을 호출하지 않는 이상, mmapped address에 매핑은 살아있구나
