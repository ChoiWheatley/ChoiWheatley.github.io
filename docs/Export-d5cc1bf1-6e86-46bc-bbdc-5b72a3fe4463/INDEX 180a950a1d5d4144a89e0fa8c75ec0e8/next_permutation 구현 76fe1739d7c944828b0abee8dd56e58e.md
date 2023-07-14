---
description:
aliases: 
tags: 
date created: Monday, February 13th 2023, 6:16:26 am
date modified: Monday, February 27th 2023, 6:20:45 pm
created: 2023-02-13T06:16:26
updated: 2023-07-11T15:21:08
title: next_permutation 구현 76fe1739d7c944828b0abee8dd56e58e
---
# next_permutation 구현

상태: In progress
태그: 순열조합

[boj.kr/17359](http://boj.kr/17359) 

> 가장 마지막에 오는 단조증가 수열을 단조감소 수열로 변환하는 문제
> 

# 예시

- [1, 2, 3] 경우의 수 = 3! = 6
    
    123
    
    132
    
    213
    
    231
    
    312
    
    321
    
- [a, b, c, d] 경우의 수 = 4! = 24
    1. abcd
    2. abdc
    3. acbd
    4. acdb
    5. adbc
    6. adcb
    7. bacd
    8. badc
    9. bcad
    10. bcda
    11. bdac
    12. bdca
    13. cabd
    14. cadb
    15. cbad
    16. cbda
    17. cdab
    18. cdba
    19. dabc
    20. dacb
    21. dbac
    22. dbca
    23. dcab
    24. dcba
    
- [a, a, b] 경우의 수 = 3! / 2 = 3
    
    aab
    
    aba
    
    baa
    

# 구현

[https://en.cppreference.com/w/cpp/algorithm/next_permutation](https://en.cppreference.com/w/cpp/algorithm/next_permutation)를 참고했다.

1. i = len(ls) - 1 ~ 0까지 거꾸로 단조 감소수열이 끝나는 인덱스 i를 찾는다.
2. upperbound자리를 찾는다. 거꾸로
3. i..<len(ls) 구간의 원소들을 뒤집는다. 왜냐면 방금 전까지는 단조감소수열이었기 때문.

```java
/**
   * 사전순 상에 다음으로 오는 조합을 collection에 저장한다.
   * 
   * @param ls         out
   * @param comparator 사전순서 정의
   * @return if has no any next permutation, return false, else, return true
   */
  public static <T extends Comparable<T>> 
  boolean nextPermutation(List<T> ls, Comparator<T> comparator) {
    final int lastIdx = ls.size() - 1;
    // 1. 단조감소수열이 끝나는 인덱스 i를 찾는다.
    int i = lastIdx;
    for (; i > 0; --i) {
      if (comparator.compare(ls.get(i - 1), ls.get(i)) < 0)
        break;
    }
    if (i == 0)
      return false;

    // 2. 이진탐색을 활용하여 i - 1에 위치한 원소와 그보다 큰 원소들 중 가장 작은 원소(upperbound)를
    // 찾아 서로 바꾼다. 참고: [i:last]는 현재 reversed이다.
    int lo = i;
    int hi = ls.size();
    T tmp = ls.get(i - 1);
    while (hi - lo > 1) {
      int mid = (lo + hi) / 2;
      if (comparator.compare(ls.get(mid), tmp) <= 0) {
        // go left
        hi = mid - 1;
      } else {
        // go right
        lo = mid;
      }
    }
    ls.set(i - 1, ls.get(lo));
    ls.set(lo, tmp);

    // 3. i번째 원소부터 단조증가수열로 만든다.
    int left = i;
    int right = lastIdx;
    while (left < right) {
      tmp = ls.get(left);
      ls.set(left, ls.get(right));
      ls.set(right, tmp);
      left++;
      right--;
    }

    return true;
  }
```

---

[boj.kr/17359](http://boj.kr/17359) 문제 해결 소스코드, 시간 순서대로 푼 코드를 지우지 않고 별도의 클래스로 처리했다. [https://www.acmicpc.net/source/53386573](https://www.acmicpc.net/source/53386573)