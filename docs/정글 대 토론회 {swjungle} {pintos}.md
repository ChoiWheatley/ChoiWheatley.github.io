---
aliases: 
tags: 
description:
title: 정글 대 토론회 {swjungle} {pintos}
created: 2023-10-12T18:35:43
updated: 2023-10-12T19:25:42
---
- [[week07-09 {swjungle} {pintos}]]
___

## README

본 파일은 2023-10-12T20:00:00에 진행되는 정글 대 토론회 (by 병철) 발표준비 + 다른 발표자들과의 대화 내용을 기록하기 위해 제작되었습니다. 저는 특별히 [[list {C}#Generic Linked List]] 발표를 준비하려고 하고 같은 리포지토리를 활용하여 발표를 진행하는 세준이는 `child_info`에 대해서 발표한다고 합니다.

## Generic Linked List

**`list_entry`** 매크로 분석

```
#define list_entry(LIST_ELEM, STRUCT, MEMBER) ((STRUCT *) ((uint8_t *) &(LIST_ELEM)->next - offsetof (STRUCT, MEMBER.next)))
```

![[스크린샷 2023-09-23 오후 2.52.01.png]]

한 줄 요약: `list_elem`타입 변수의 outer structure를 참조하기 위해서 구조체의 leaf-member의 offset을 구해 그만큼의 바이트를 빼줍니다.

- 왜 `&(LIST_ELEM)->next`를 `uint8_t *`로 캐스팅 하나요? ⟶ 그건 `uint8_t`의 보폭이 8비트, 즉 1바이트이기 때문입니다.
- 왜 `->next`인건가요? ⟶ 그건 leaf member로 지정하기 위해서입니다. 따라서 `->prev` 해도 동일한 결과를 얻습니다. 

**도식**

[[Drawing 2023-10-12 18.54.09.excalidraw]]  
![[Pasted image 20231012191555.png]]

위의 타입에 대하여 `offsetof(struct thread, elem.next)`의 값은 `elem::next`가 시작하는 오프셋인 36이다. `list_elem` 타입 입장에서 `next`의 오프셋은 8인데 거기에서 36을 뺀 것이므로 `elem`타입 입장에선 `&elem + 8 - 36` = `&elem - 28`이 될 것이고, 이는 곧 `struct thread`의 출발주소가 된다.

[[Drawing 2023-10-12 19.22.01.excalidraw]]  
![[Pasted image 20231012192414.png]]

`elem->next`의 값은 110, `&elem->next`의 값은 18이다. 따라서 `&(LIST_ELEM)->next`를 할지라도 다음 `list_elem`을 참조하는 것이 아니라는 점 유의하자.

## 실수 복기
