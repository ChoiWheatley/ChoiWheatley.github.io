---
aliases: 
tags: 
description:
title: list {C}
created: 2023-09-20T14:38:29
updated: 2023-09-30T15:25:35
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
