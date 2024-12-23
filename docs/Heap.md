---
description:
title: Heap
created: 2023-02-10T11:39:15
categories: 
 - 알고리즘
 - 힙정렬
 - 최대힙
 - 최소힙
aliases: 
 - 힙
 - heap
tags: [" algo/heap ", algo/heap]
date created: Friday, February 10th 2023, 11:39:15 am
date modified: Monday, February 27th 2023, 6:20:45 pm
updated: 2024-01-11T00:48:11
---
parent link: [[0011 Algorithms ♾️|algorithms]] [[SW Expert Academy|swea]]

---

# 관련 문제 링크

- [x] [[no23.힙 (class Heap 소스코드)]]
- [x] [[기초 partial sort 연습]]
	- [x] [[Pro 사전문제 - 기초 Partial Sort 연습]]
- [x] [[No. 24 - 보급로]]
- [x] [[No. 25 중간값 구하기]]
- [ ] [[1181 단어 정렬]]
- [ ] [[11279 최대 힙 {boj} {heap} {python class Heap}]]
- [x] [[1655 가운데를 말해요 {boj} {heapq}]]

___

# 개요

complete binary tree로, 배열로 구현하면 자식노드는 본인 인덱스의 두배가 된다. 빠르게 구현하는 것이 목적이므로 0번 노드를 일부러 사용하지 않는것도 도움이 된다. 

- 0부터 시작하는 경우
	- 부모의 인덱스 :  `(i-1) >> 1`
	- 왼쪽 자식의 인덱스 : `i << 1 | 1`
	- 오른쪽 자식의 인덱스 : `i << 1 + 2`  
- 1부터 시작하는 경우
	- 부모의 인덱스 : `i >> 1`
	- 왼쪽 자식의 인덱스 : `i << 1`
	- 오른쪽 자식의 인덱스 : `i << 1 | 1`

Max heap: 루트가 가장 큼. 부모 key > 자식 key  
Min heap: 루트가 가장 작음. 부모 key < 자식 key

~~참고 : Key 중복은 허용하지 않음.~~  
2023-02-10 21:53 수정: 중복 허용함 [wiki](https://en.wikipedia.org/wiki/Heap_%28data_structure%29)

# insert

마지막 노드에 추가 후 부모와의 비교를 하며 **bubble up**

```cpp
  auto insert(T &&data) {
    if (m_top >= N - 1) {
      throw std::out_of_range{"heap is full(" + std::to_string(N) + " size)"};
    }
    // push to top
    m_top++;
    m_data.at(m_top) = std::move(data);

    // bubble up
    for (size_t idx = m_top;
         parent(idx) > 0 && //
         m_less_than(m_data[parent(idx)], m_data[idx]);) {
      std::swap(m_data[parent(idx)], m_data[idx]);
      idx = parent(idx);
    }
  }

```

# delete

오직 루트만을 삭제할 수 있음. 루트 삭제 후 마지막 노드를 루트에 올려넣고 자식과 비교하며 더 큰 값 (min heap일 경우 더 작은 값)과 비교하며 **bubble down**

```cpp
  void pop() noexcept {

    if (empty())
      return;

    // push last element
    m_data[START] = std::move(m_data[m_top]);
    m_data[m_top] = T{};
    m_top--;

    for (size_t idx = 1; left(idx) <= m_top;) {
      size_t next_idx = 0;

      // find next index
      if (left(idx) == m_top) {
        next_idx = left(idx);
      } else {
        auto const &left_one = m_data[left(idx)];
        auto const &right_one = m_data[right(idx)];
        if (m_less_than(left_one, right_one)) {
          next_idx = right(idx);
        } else {
          next_idx = left(idx);
        }
      }

      // bubble down
      if (m_less_than(m_data[idx], m_data[next_idx])) {
        std::swap(m_data[idx], m_data[next_idx]);
      } else {
        break;
      }

      idx = next_idx;
    }
  }

```

pop이 구현하는데 꽤나 고생했다. left, right 인덱스가 자꾸 예상을 벗어난 위치에 도달했기 때문. 
1. 첫번째 for문에서 `left(idx) <= m_top` : 다음의 경우의 수가 가능하다.
	1. `left(idx) == m_top` : 오직 왼쪽 자식으로만 내려갈 수 있다.
	2. `right(idx) <= m_top` : 왼쪽, 오른쪽 자식 중에서 비교를 통해서 하나를 골라야 한다.
2. bubble down 코드에서 자식 노드와 비교 후 내려갈지 여기에서 멈출지를 선택해야 한다.

# 아무 정렬문제 힙으로 풀기 (python)

힙소트가 아니라 '힙' 클래스를 구현하여 풀었다는 점에 의의를 두었다.  
[1181 {boj} source code](https://www.acmicpc.net/source/65079282) | [[1181 단어 정렬]]
