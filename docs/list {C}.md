---
aliases: 
tags: 
description:
title: list {C}
created: 2023-09-20T14:38:29
updated: 2023-10-12T19:57:20
---
**depends on**: [[mymalloc.h {C}]]

```c
#pragma once
#include <mymalloc.h>
#include <stddef.h>
/**
 * C는 아직 존재하지 않는 타입에 대한 포인터 생성을 지원한다. p.160
 */
typedef struct Node* NodePtr;

/// @brief 단일연결리스트 원소를 정의.
typedef struct Node {
  int data;
  NodePtr link;
} Node;

/// @brief first 체인의 앞에 data를 가진 노드를 추가한다.
/// @param first NULLable한 타입이기 때문에 first의 주소를 인자로 받는다.
void insert_left(NodePtr* first, int data) {
  NodePtr newnode;
  MALLOC(newnode, sizeof(*newnode));
  newnode->data = data;

  if (!(*first)) {
    // first is empty, first is now data
    newnode->link = NULL;
    *first = newnode;
    return;
  }

  NodePtr oldnode = (*first);
  newnode->link = oldnode;
  *first = newnode;
}

/// @brief first가 가리키고 있는 데이터를 연결해제 시킨 뒤 리턴한다.
/// @param first nullable, 연결리스트의 첫번째 원소를 가리키는 포인터.
/// @return first가 가리키고 있는 첫번째 원소 혹은 NULL을 리턴
NodePtr pop_left(NodePtr* first) {
  if (!(*first)) {
    return NULL;
  }
  NodePtr trail = (*first)->link;
  NodePtr ret = (*first);
  (*first) = trail;
  return ret;
}

/// @brief 순회를 하며 하나씩 출력한다.
/// @param first nullable, 연결리스트의 첫번째 원소를 가리키는 포인터
void print_list(const NodePtr first) {
  if (!first) {
    printf("[]\n");
    return;
  }
  printf("[");

  for (NodePtr cur = first; cur; cur = cur->link) {
    printf("%d", cur->data);
    if (cur->link) {
      printf(", ");
    }
  }

  printf("]\n");
}

/// @brief list의 노드 수를 반환하세요
/// @param first nullable, 연결리스트의 첫번째 원소를 가리키는 포인터
int __len__(const NodePtr first) {
  int cnt = 0;
  for (NodePtr cur = first; cur; cur = cur->link) {
    cnt += 1;
  }
  return cnt;
}
```

## Generic Linked List

- [lib/kernel/list.c](https://github.com/ChoiWheatley/swjungle-week07-09/blob/master/lib/kernel/list.c), [lib/kernel/list.h](https://github.com/ChoiWheatley/swjungle-week07-09/blob/master/include/lib/kernel/list.h)
	- 순회, empty 여부, list size, 원소 삭제 (임의 위치, front, back), 원소 삽입 (front, back, ordered), type definition of less predicate function, sort, unique, max, min에 대한 구현이 들어가 있다.
	- `list_entry` 매크로 함수를 사용한 outer structure 획득 

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
