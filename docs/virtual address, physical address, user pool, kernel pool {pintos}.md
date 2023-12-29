---
aliases: 
tags: 
description:
title: virtual address, physical address, user pool, kernel pool {pintos}
created: 2023-10-13T17:43:57
updated: 2023-10-14T04:33:13
---
- [[week07-10 {swjungle} {pintos}]]
___

## `pml4e_walk`

4단계 이터레이션마다 테이블 생성(palloc) 후 인자로 받은 커널 가상주소(kva)를 `vtop`하여 entry의 인덱스에 집어넣고 있다.

- `upage` 언제 생성되는가? ⟶ 생성된다기 보다는 그 자리에 이미 있을 거라고 가정한다고 보는 편에 가깝지않나?
	- 스택영역: 지역변수, 함수의 위치는 이미 컴파일 단계에서 결정이 난 상황.
	- 동적영역: palloc, malloc을 통해서 생성되는 kva(kernel virtual address)는 유저 프로세스가 호출할 수 없는 상태. 다만 유의할 점이 `mmap` 시스템 콜은 유저가 호출할 수 있기에 결국 kva를 uva(user virtual address)로 변환해 주는 코드가 필요하다.
- `kpage`: KERN_BASE 위에 있다. palloc 결과가 이 친구임. vtop를 하면 frame으로 변환가능.
