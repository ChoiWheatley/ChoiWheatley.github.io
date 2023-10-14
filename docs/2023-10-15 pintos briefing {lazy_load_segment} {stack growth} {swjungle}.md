---
aliases: 
tags: 
description: 본 문서는 lazy load segment 구현 완료로 인해 테스트 코드를 실행해 볼 수 있게 되어 큰 산 넘었다고 판단하여 진행한 브리핑의 내용을 요약했습니다.
title: 2023-10-15 pintos briefing {lazy_load_segment} {stack growth} {swjungle}
created: 2023-10-15T02:32:23
updated: 2023-10-15T04:39:29
---
- [[week07-09 {swjungle} {pintos}]]
- [Pull Request Link]()
___

## README

본 문서는 lazy load segment 구현 완료로 인해 테스트 코드를 실행해 볼 수 있게 되어 큰 산 넘었다고 판단하여 진행한 브리핑의 내용을 요약했습니다.

- 우리 위치는? 문서 이해도, 코드 이해도, 테스트 케이스 현황의 기준으로
- WIL (Weekly I Learned) lazy loading 플로우, 인텔 공식 문서의 중요성, 정리의 타이밍, callback과 함수 포인터와의 관계

## 우리 위치는?

### 지식의 위치는?

- 문서 이해도: 낮은 편, 코드 분석이 먼저였고, hash, spt, pml4 api 이해도 없이 박치기를 수행했다.
	- supplement page table 구현에 필요한 함수 `spt_find_page`, `spt_insert_page`, `spt_remove_page` 정도만 인지하고 시작함. 그래서 `load_segment`, `lazy_load_segment`, `vm_alloc_page_with_initializer`, `uninit_new`, `claim` 등 demand paging에 관여하는 다양한 함수들의 존재를 뒤늦게 알게 되었다.
- 코드 이해도: 말해뭐해. 하루종일 코드만 봤는데. 함수들의 유스케이스만큼은 확실하게 파악했다.

### What We Have Done

- **lazy loading**: 
	- spt에 uninit 페이지 구조체 등록, 
	- page fault 발생시 spt find 및 frame 생성, 
	- 유저 가상 메모리와 커널 가상메모리 연결 (pml4-manner, spt page manner)
	- `aux`에 파일 메타데이터 담아서 lazy loading 수행.
- **stack growth**: 
	- 단순히 claim만 호출하는 것으로 기본 구현 달성.
- **anonnymous memory(VM_ANON)**:
	- 바이너리를 위한 페이지는 `VM_ANON`으로 설정되어있어 기본적인 코드만 추가함. `anon_initializer`만 급히 구현했고 `anon_swap_*`, `anon_init` 파트는 비어있다.

### 눈여겨 봐야 할 Test Cases

- Project1
	- all pass
- Project2
	- args many가 왜 터짐??? stack 용량부족?
	- read boundary, write normal는 거들떠도 안 봤는데...
	- fork/exec/wait/rox 시리즈는 터질 수밖에 없다. 왜냐하면 얘네들은 stack growth도 아니고 새로운 컨텍스트를 lazy하게 만들어야 하기 때문. eviction policy도 Fail에 일조하지 않았을까?
	- syn read, syn write 확정적으로 터진다. 10번 중 2~3번 터지던 Project2의 상황보다는 낫다고 판단해야하나?
- Project3
	- pt (page table) 시리즈는 구현하다 만 부분이 많기 때문에 어떤 테스트는 통과하지만 어떤 테스트는 실패한다.
	- page 시리즈
	- mmap 시리즈
	- COW 메커니즘

## WIL에 추가해주세요

- `alloc_with_initializer`: 타입에 따라서 **동적으로** annonymous, file-backed 페이지를 정의한다는 것이 놀라웠다.
	- 첫 page fault 담당일진 `uninit_initialize`는 `uninit_new`에서 spt에 페이지 구조체를 등록할 때 넣어주었던 페이지 타입에 따라 `initializer`를 호출하며 그 안에서 `operators` 멤버를 갱신한다. `uninit_initializer`는 page fault 발생시 수행할 루틴을 `init` 안에 담아두었기 때문에 예를 들어 `process_exec`의 경우, 최초로 생성될 스택영역을 프레임으로 받아다 연결해주는 `lazy_load_segment`를 호출하게 된다.
- **`lazy_load_segment`에서 오프셋 `ofs`를 advance해야한다는 사실을 알았다.** Project2 `load_segment`에서는 연속적으로 `read`를 호출하기 때문에 오프셋이 자동으로 advance 했지만 이번 프로젝트부터는 한 번에 한 페이지씩 `lazy_load_segmenet`를 등록했기 때문에 매번 seek를 해야한다. 쉽게 말하자면, text, data, bss 영역 등을 로드하는 과정은 이중 반복문을 통해 이루어진다. elf 헤더에 등록된 섹션들을 순회하느라 한 번, 그 안에 탑재된 블럭들을 읽고 적재하느라 한 번. 그런데 두 번째 반복문에서 원래 이루어졌어야 할 loading이 이루어지지 않고 `vm_alloc_page_with_initializer`를 사용하여 spt에 루틴과 몇가지 인자들을 넣어 등록만 할 뿐이다. 따라서 `lazy_load_segment`가 호출될 당시는 해당 페이지를 접근하려다 page fault가 발생하여 발생한 문제를 해결하기 위해 출범한 `try_fault`에 의해서일 뿐이고 이 타이밍은 file 구조체의 오프셋을 절대로 믿어선 안될때이다. 따라서 `load_segment`에서 헤더 메타데이터들을 `aux`에 담아서 spt에 등록할 때 들어간 `ofs`를 기준으로 seek를 해야한다.
- **intel, System V ABI 공식문서의 중요성!** 신뢰도 낮은 글을 철썩 믿어버리지 말고 차라리 공식문서를 찾아봐라.
- **정리의 타이밍**: 문서를 읽다보면 발견하는 모르는 키워드 위주로. no blog, 추론 먼저, 공식문서를 사용하여 검증.

**더 알고싶어요**

- lazy loading 내지는 demand paging 기법을 구현하면서 마주한 `page::operators::swap_in, swap_out, destroy` 함수 포인터 인터페이스(??)를 다루는 방법을 더 알고 싶어요.
- union이 모던 프로그래밍에서도 쓰이는지 알고 싶어요.

## 앞으로 할 일 및 목표

WEEK09 전까지 mmap에 발 들여놓고 플로우 정도는 파악하는 것을 목표로 해보자!

1. demand paging메커니즘을 사용하여 **fork/exec** 시리즈를 통과하는 것이 먼저일 것 같다. 처음부터 Project 1/2 통과하려고 한다면 금방 demand paging이 필요하다는 것을 알게 될 것이다.
2. 그 뒤에는 vm 시리즈를 우선적으로 해결하자.
3. userprog 시리즈는 1,2를 수행하면서 자연스럽게 해결될 수도 있을 것이다. 마지막까지도 바짓가랑이를 붙잡고 늘어진 테스트 케이스를 해치우자.

**자원 해제**

`malloc`, `palloc` 했던 수많은 페이지 구조체들을 아직 해제하지 않고있다..!!! 

## Miscellaneous

- `pml4_set_page`를 우리팀은 `claim_page`에서, 동호네는 `anon_initializer`에서 수행한다고 한다. 참고해보자.
