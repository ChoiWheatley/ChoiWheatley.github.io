---
description:
aliases: 
tags: 
date created: Monday, February 13th 2023, 6:16:28 am
date modified: Monday, February 27th 2023, 6:20:45 pm
created: 2023-02-13T06:16:28
updated: 2023-07-11T15:21:08
title: lowerbound
---
# lowerbound

상태: In progress
태그: search
[notion에 더 자세한 설명이 있어요](https://choiwheatley.notion.site/lower-upper-bound-parametric-search-e7e3716351934d7fbeda02ca26c70f53)
[boj.kr/1205](http://boj.kr/1205) [boj.kr/25947](http://boj.kr/25947) 

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