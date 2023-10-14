---
aliases: 
tags: 
description:
title: 2023-10-15 pintos briefing {lazy_load_segment} {stack growth} {swjungle}
created: 2023-10-15T02:32:23
updated: 2023-10-15T03:10:32
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
	- 
