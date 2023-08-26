---
description: 이진탐색에 활용하는 lower bound를 파이썬에서는 `bisect.bisect_left`로 가능하다.
aliases: 
tags: 
created: 2023-05-18T17:32:42
updated: 2023-08-11T13:40:39
title: lower bound with bisect_left (python)
---
[`bisect.bisect_left`](https://docs.python.org/3/library/bisect.html#bisect.bisect_left)

C++에 [[이분탐색을 활용한 lowerbound 구현법 {Algo}|lower_bound]]가 있다면 파이썬에는 `bisect.bisect_left`가 있다. 그럼 `upper_bound`는? `bisect.bisect_right` 바보야 😲

이분탐색은 그 자체로 너무 많은 곳에 활용이 되고 있어서 실전 위주로 정리를 해보겠다. 흠흠

아래는 [[LIS 가장 긴 증가하는 부분수열]]를 파이썬으로 구현한 코드이다. 확실히 엄청나게 간결하다.

![[20230518 estsoft - python - tree -- LIS -- selection sort -- insertion sort -- merge sort -- quick sort#^4ygbua]]

아래는 삽입정렬을 `bisect_left`를 활용하여 푼 코드이다. 부분정렬리스트인데, 이분탐색을 안할 이유가 없잖아?

![[20230518 estsoft - python - tree -- LIS -- selection sort -- insertion sort -- merge sort -- quick sort#^fyzoll]]
