---
aliases: 
tags: 
description:
title: TLB Hit, Miss의 과정을 순차적으로 설명해주세요
created: 2024-01-18T19:52:38
updated: 2024-01-18T21:28:45
---
- [[9.6. Address Translation]] 
---

## TLB Hit

CPU가 가상주소에 해당하는 값을 읽어오라는 명령을 MMU에게 합니다. MMU는 제일먼저 가상주소 뒤의 비트들을 통해 TLB 인덱스와 태그를 알아낸 뒤에 해당 위치에 Validation bit가 켜져있는지 확인합니다. 켜져있는 경우 거기에 적혀있는 물리페이지 번호를 통해 캐시메모리, 물리메모리에 들어있는 값을 확인합니다.

## TLB Miss

TLB 인덱스와 태그에 해당하는 validation bit가 꺼져있는 경우입니다. MMU는 TLB Miss에 대응하는 하드웨어 인터럽트를 발생시키고, 인터럽트 핸들러는 페이지 테이블을 참조해 PTE에 적혀있는 물리주소를 가져오고, 이것으로 캐시메모리, 물리메모리에 들어있는 값을 확인합니다.
