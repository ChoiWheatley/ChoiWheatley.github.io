---
aliases: 
tags: 
description:
title: realloc {C}
created: 2023-09-02T13:43:35
updated: 2023-09-02T13:43:59
---

realloc을 수행하면 기존의 주소값을 버리고 새 주소로 이주를 하는 것인가, 복사를 하는 것인가? 

- [cppreference](https://en.cppreference.com/w/c/memory/realloc)에 따르면, 기존 영역을 확장/축소 가능한 경우 별도로 free를 하지 않는다. 불가능한 경우 별개의 메모리공간을 다시 할당한 뒤 새 크기와 이전 크기 중 작은 크기의 메모리 영역을 복사하고 이전 영역을 삭제한다.

> The original pointer `ptr` is invalidated and any access to it is UNDEFINED BEHAVIOR (even if reallocation was in-place)  
> 기존 포인터는 재할당이 제자리에서 일어났을지라도 비활성화 되고 기존 포인터에 대한 접근은 정의되지 않은 동작입니다.
