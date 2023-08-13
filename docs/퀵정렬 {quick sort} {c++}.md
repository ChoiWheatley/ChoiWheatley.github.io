---
description:
aliases: Quick Sort
created: 2023-02-27T16:43:12
updated: 2023-08-13T21:47:45
tags:
  - notion
  - algo/sort
title: 퀵정렬 {quick sort} {c++}
---
<https://choiwheatley.notion.site/quick-sort-8720f36d603840f1b37fa73465c76eb0>

# TL;DR

- 퀵정렬은 분할정복의 예시 중 유명한 알고리즘으로, 병합정렬의 **분할 후 정복** 개념과는 달리 **정복 후 분할** 정책을 사용한다.
- 먼저 시퀀스를 두 부분으로 나누는 **파티션** 작업을 통해 피벗의 위치를 고정시킨다. 그리고 **재귀호출**을 사용하여 피벗의 왼쪽과 오른쪽에 대한 정렬을 각기 수행한다.
- 피벗의 위치는 맨앞, 맨뒤, 정 가운데, 랜덤, 중앙값으로 잡는 방법이 있고, 아래 수도코드는 랜덤으로 인덱스를 잡아 end로 보낸 뒤 파티셔닝을 수행한다.

# Pseudo Code

유튜브 영상에서 보던 방식과는 조금 달라서 헤맸다. 실제 코드를 보면 two pointer개념을 사용하기는 하지만 pivot을 기점으로 물리적으로 분리된 공간에 실제로 데이터를 swap하는 것이 보였으나, 여기 방식에 따르면 일단 한쪽 끝 (아래 코드는 end)으로 피벗을 배치시킨 뒤에 pivot보다 작은 원소의 개수를 카운트 하고 incremental 하게 위치를 잡는 것을 볼 수 있다.

맨 마지막 줄이 중요한데, 피벗의 위치는 아직 끝이므로 본래 있어야 할 자리로 옮기는 것을 알 수 있다.

```c++
#include <random>
#include <utility>
static std::mt19937 engine{std::random_device{}()};
static std::uniform_int_distribution<unsigned int> generator{};

template <typename Iter, class Less>
Iter partition(Iter begin, Iter end, Less const &less) {
  auto pivot = begin + generator(engine) % (end - begin);
  std::swap(*pivot, *(end - 1));
  pivot = end - 1; // ==IMPORTANT== pivot is currently end of the sequence
  size_t count = 0; // pivot보다 작은 원소의 개수를 카운트
  for (auto cur = begin; cur != pivot; ++cur) {
    if (less(*cur, *pivot)) {
      // move element to left
	  // begin ~ begin + count => *pivot 보다 작은 원소들
	  // begin+count ~ cur => *pivot 보다 크거나 같은 원소들
      std::swap(*cur, *(begin + count));
      count += 1;
    }
  }
  // move pivot to right place
  std::swap(*pivot, *(begin + count));
  return begin + count;
}
```

```c++
template <typename Iter, class Less>
void sort(Iter begin, Iter end, Less const &less) {
  if (end - begin <= 1) {
    return;
  }
  auto mid = partition(begin, end, less);
  sort(begin, mid, less);
  sort(mid + 1, end, less);
}
```

# pseudocode (python)

```python
from typing import MutableSequence, Callable
from random import randint

def partition(ls: MutableSequence, lo: int, hi: int, less: Callable[[int, int], bool]):
    pivot_idx = randint(lo, hi - 1)
    # move pivot last of the element
    ls[pivot_idx], ls[hi - 1] = ls[hi - 1], ls[pivot_idx]
    pivot_idx = hi - 1

    count = 0  # count will be the separating point between less(x, pivot) and other
    for i in range(lo, pivot_idx):
        # iterate EXCEPT pivot
        if less(ls[i], ls[pivot_idx]):
            ls[i], ls[lo + count] = ls[lo + count], ls[i]
            count += 1
    # move pivot to the right place
    ls[pivot_idx], ls[lo + count] = ls[lo + count], ls[pivot_idx]
    return lo + count


def qsort(ls: MutableSequence, lo: int, hi: int, less: Callable[[int, int], bool]):
    """
    - lo: starting index (inclusive)
    - hi: end index (exclusive)
    - less: function that can customize ordering
    """
    if hi - lo <= 1:
        return
    pivot = partition(ls, lo, hi, less)
    qsort(ls, lo, pivot, less)
    qsort(ls, pivot + 1, hi, less)  # pivot은 넣으면 무한재귀
```

# 관련문제

- [[염라대왕의 이름정렬]]
