---
aliases: 
tags: algo/binarysearch 
description:
title: binary search를 활용한 lower upper bound 그리고 parametric search까지 {Notion export}
created: 2023-08-12T04:47:40
updated: 2023-08-18T09:44:43
---

# lower/upper bound 그리고 parametric search까지

태그: binarysearch  
상태: 정리완료  
최종 편집 일시: 2023년 3월 2일 오후 7:25

# TL;DR

- [std::lower_bound {c++}](https://en.cppreference.com/w/cpp/algorithm/lower_bound): 구간 `[first, last)` 에서 `element < value`를 만족하는 첫번째 `element`를 찾는 알고리즘
- [std::upper_bound {c++}](https://en.cppreference.com/w/cpp/algorithm/upper_bound) :  구간 `[first, last)`에서 `value < element`를 만족하는 첫번째 `element`를 찾는 알고리즘
- 정렬된 리스트 보다는 주어진 검색결과 (혹은 함숫값)가 True, False 구간으로 나뉘어있을 때 그 경계를 찾는 문제라고 보는 게 더 낫다.
- 그러면 검색키 $x$가 주어졌을 때  lower_bound 문제의 first true 조건은 $v \ge x$ 를 만족하는 첫번째 v를 찾는 문제가 되고
- upper_bound 문제의 first true 조건은 $v \gt x$를 만족하는 첫번째 v를 찾는 문제가 된다.
- 아래 표는 어떤 정렬된 배열에서 `value` = 20에 대한 lower_bound와 upper_bound를 찾은 모습이다.

| 10 | 20 | 20 | 20 | 30 | 40 |  
| ---| --- | --- | --- | --- | --- |  
| F  | F | F |  | |  |
|  | lower_bound |  |  | upper_bound |  |

# 구현방법

실수의 여지가 오지게 많다. 꼼꼼하게 살펴볼 필요가 있다. 실수를 줄이기 위해서 다음과 같은 마인드로 구현해보자.

- IF 문은 한 번만 쓰는 게 좋다. v < x, v == x, v > x 이따위로 조건문 세개로 나누는 건 이분탐색에 어울리지 않아!
- 중간값(`m`)에 대한 함숫값이 참인 경우 ⇒ `[m, r)` 범위는 볼 필요가 없다. 따라서 `[l, m)` 범위를 탐색하면 된다.
- 중간값 `m`에 대한 함숫값이 거짓인 경우 ⇒ `[l, m+1)` 범위는 볼 필요가 없다. 따라서 `[m+1, r)` 범위를 탐색하면 된다.

이런 식으로 소거법으로 정리해 나간다는 느낌으로 코드를 짜면 언제 반복문을 나가야 할지, 언제 l,r을 바꿔야 할지 알 수 있다.

# pseudo code

```python
def first_true(begin, end, pred):
	l = begin
	r = end
	while l != r :
		m = l + (r - l) / 2 # overflow 방지
		match pred(m):
			case True:
				r = m 
			case _:
				l = m + 1
	return l
```

```cpp
/**
F-...-F-F-F-T-T-T-...-T
            ^
*/
template <typename T, class Predicate>
inline T first_true(T begin, T end, Predicate const &pred) {

  T l = begin;
  T r = end;
  while (l < r) {
    T mid = l + (r - l) / 2;
    if (pred(mid)) {
      // next range is [l, mid)
      r = mid;
    } else {
      // next range is [mid + 1, r)
      l = mid + 1;
    }
  }
  return l;
}
```

`first_true`로 구현한 `upper_bound`

```cpp
TEST(BinSearch, UpperBound) {
  vector<int> sorted = {1, 3, 5, 5, 5, 7, 9};
  int key = 5;
  auto upper_b = first_true(sorted.begin(), sorted.end(),
                            [key](auto e) { return key < *e; });
  auto real_upper_b = std::upper_bound(sorted.begin(), sorted.end(), key);
  ASSERT_EQ(real_upper_b, upper_b);
}
```

`first_true`로 구현한 `lower_bound`

```cpp
TEST(BinSearch, LowerBound) {
  vector<int> sorted = {1, 3, 5, 5, 5, 7, 9};
  int key = 5;
  auto lower_b = first_true(sorted.begin(), sorted.end(),
                            [key](auto e) { return key <= *e; });
  auto real_lower_b = std::lower_bound(sorted.begin(), sorted.end(), key);
  ASSERT_EQ(real_lower_b, lower_b);
}
```

# Parametric Search로의 확장

> 이진탐색은 최적화 문제를 결정 문제로 바꿔서 풀 수 있게 해준다.
> 

- 최적화 문제 = $f(x)=\text{True}$가 되게 하는 x의 최댓값을 구하여라.
- 결정 문제 = 어떤 x에 대하여 $f(x)=\text{True}$인가?

다만 이 f라는 녀석에 조건이 필요한데, 아래 식을 만족하는 함수 f는 함수값이 불리언 자료형에 의해 정렬된 형태이기 때문에 이분탐색이 가능해진다.

$$
x_1 \lt x_2\\ f(x_1)=\text{True} \Rightarrow f(x_2)=\text{True}
$$

# Legacy

lowerbound는 정렬된 리스트에서 찾고자 하는 검색키보다 “작지 않은” 원소들 중 첫번째 원소를 가리킨다. 대표적으로 C++의 std::lowerbound 가 있다. 반대로 upperbound라는 개념도 있는데, 얘는 검색키보다 “큰” 원소들 중에서 첫번째 원소를 가리킨다. 즉, equal한 원소를 포함하지 않는다.

lowerbound는 어느 상황에서 쓰이는가? 바로 중간에 원소를 삽입할 일이 있을 때 요긴하게 사용된다. lowerbound는 새 key값을 삽입해야 하는 상황에서 바로 리스트에 집어넣을 인덱스를 리턴하기 때문에 유용하다.

# lowerbound 시뮬레이션

| key | 10 | 20 | 20 | 20 | 50 | end |
| --- | --- | --- | --- | --- | --- | --- |
| 5 | ^ |  |  |  |  |  |
| 10 | ^ |  |  |  |  |  |
| 20 |  | ^ |  |  |  |  |
| 25 |  |  |  |  | ^ |  |
| 55 |  |  |  |  |  | ^ |

1. 5보다 작지 않은 원소들: {10,20,20,20,50, end} 중 첫번째 원소의 인덱스 = 0
2. 10보다 작지 않은 원소들: {10,20,20,20,50, end} 중 첫번째 원소의 인덱스 = 0
3. 20보다 작지 않은 원소들: {20,20,20,50,end} 중 첫번째 원소의 인덱스 = 1
4. 25보다 작지 않은 원소들:{50, end} 중 첫번째 원소의 인덱스 = 4
5. 55보다 작지 않은 원소들: {end} 중 첫번째 원소의 인덱스 = 5 == length of list

# upperbound 시뮬레이션

| key | 10 | 20 | 20 | 20 | 50 | end |
| --- | --- | --- | --- | --- | --- | --- |
| 5 | ^ |  |  |  |  |  |
| 10 |  | ^ |  |  |  |  |
| 20 |  |  |  |  | ^ |  |
| 25 |  |  |  |  | ^ |  |
| 55 |  |  |  |  |  | ^ |

# pseudo code

구현 방법으로는 바이너리 서치가 있다. 

```python
def lower_bound(ls: [int], val: int) -> int: 
  # todo
```
