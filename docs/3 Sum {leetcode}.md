---
aliases: 
tags: 
description:
title: 3 Sum {leetcode}
created: 2023-10-29T22:41:57
updated: 2023-10-29T23:46:09
---
- [[week12 {swjungle}{ALGORITHMS}]]
- [1트](https://leetcode.com/problems/3sum/submissions/1086834456): nums를 정렬하기만 하면 `ret[-1]`만 봐도 좋을거라고 생각했었다. 쳐맞기 전까지는.
- [2트](https://leetcode.com/problems/3sum/submissions/1086848632?source=submission-noac): 그래서 리스트를 정렬하고 set에 집어넣기로 결정했다. TLE가 발생했다.
- [3트 성공](https://leetcode.com/problems/3sum/submissions/1086869026?source=submission-noac): [참고 솔루션](https://leetcode.com/problems/3sum/solutions/3109452/c-easiest-beginner-friendly-sol-set-two-pointer-approach-o-n-2-logn-time-and-o-n-space/?source=submission-noac)단순 combinations를 활용하기보다는 j, k에 대한 two pointers로 적극적으로 sum 값을 0에 수렴하도록 만드는 것이 중요한 문제였다.
