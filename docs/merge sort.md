---
aliases: 
tags: 
description:
title: merge sort
created: 2024-01-07T22:54:58
updated: 2024-01-07T22:58:25
---
- [[20230518 estsoft - python - tree -- LIS -- selection sort -- insertion sort -- merge sort -- quick sort]]
- [[0011 Algorithms ♾️|algorithms]]

전형적인 분할정복인데, C++에서의 이터레이터 지옥과 달리 슬라이스 문법으로 아주 깔끔하게 문제를 정복할 수 있었다.

```python
def merge_sort_recur(ls) -> list:
    if len(ls) <= 1:
        return ls

    # divide
    mid = len(ls) // 2
    left = ls[:mid]
    right = ls[mid:]

    left = merge_sort_recur(left)
    right = merge_sort_recur(right)

    # conquer
    ret = []
    while left and right:
        # 둘 다 있을 때에만 이 작업을 수행한다
        if left[0] < right[0]:
            ret.append(left.pop(0))
        else:
            ret.append(right.pop(0))

    ret += left
    ret += right

    return ret


ls = [199, 22, 33, 12, 32, 64, 72, 222, 233]
assert sorted(ls) == merge_sort_recur(ls)

```

+2023-08-13T19:47:44 추가 병합하는 과정을 빠르게 하고 싶다면 [`heapq.merge()`](https://docs.python.org/3/library/heapq.html#heapq.merge) 를 사용할 수 있다. `left`, `right` 모두 정렬이 되어있다는 보장이 있으므로, 두 힙을 병합하는 것으로 목표를 달성할 수 있다.

## C++ impl

**merge**

```cpp
#include <functional>
#include <iterator>
#include <vector>
using std::vector;

template <typename Iter, class Less = std::less<>>
void merge(Iter begin, Iter end, Less const &less) {

  using value_type = typename std::iterator_traits<Iter>::value_type;

  auto temp = vector<value_type>{};
  auto mid = begin + (end - begin) / 2;
  auto l = begin;
  auto r = mid;
  while (l != mid && r != end) {
    if (less(*l, *r)) {
      temp.push_back(*l);
      l++;
    } else {
      temp.push_back(*r);
      r++;
    }
  }
  while (l != mid) {
    temp.push_back(*l);
    l++;
  }
  while (r != end) {
    temp.push_back(*r);
    r++;
  }
  // paste it into original
  for (auto itr = begin; itr != end; ++itr) {
    *itr = temp[itr - begin];
  }
}
```

**divide**

```cpp
template <typename Iter, class Less>
void merge_sort(Iter begin, Iter end, Less const &less) {
  if (end - begin <= 1) {
    return;
  }
  auto mid = begin + (end - begin) / 2;
  merge_sort(begin, mid, less);
  merge_sort(mid, end, less);
  merge(begin, end, less);
}
```
