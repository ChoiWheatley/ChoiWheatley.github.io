---
aliases: 
tags: 
description:
title: 2812 크게 만들기 {boj} {stack}
created: 2023-08-18T16:11:30
updated: 2023-08-18T16:14:27
---
<https://www.acmicpc.net/problem/2812>

## 이 문제의 풀이에서 시간초과가 날 법한 부분을 고르시오.

```python
def sol(n: int, k: int) -> int:
    """n에서 k개의 숫자를 지웠을 때 얻을 수 있는 가장 큰 수를 출력한다"""
    stack = []
    for digit in [int(c) for c in str(n)]:
        while k > 0 and len(stack) > 0 and digit > stack[-1]:
            k -= 1
            stack.pop()
        stack.append(digit)
    while k > 0:
        k -= 1
        stack.pop()
    return int("".join([str(x) for x in stack]))


_, k = [int(x) for x in input().split()]
n = int(input())

print(sol(n, k))
```

## `str`, `int` 형변환에서 시간을 많이 잡아먹는다.

위의 코드는 input할 때 한 번, `sol` 함수 루프 돌 때 한 번, 리턴할 때 N번 형변환을 수행한다. 이 부분을 모두 덜어내니 바로 1832ms에서 220ms로 확 줄었다. 아래는 형변환 없이 통과한 코드이다.

```python
def sol(n: str, k: int) -> str:
    """n에서 k개의 숫자를 지웠을 때 얻을 수 있는 가장 큰 수를 출력한다"""
    stack = []
    for digit in (int(x) for x in n):
        while k > 0 and len(stack) > 0 and digit > stack[-1]:
            k -= 1
            stack.pop()
        stack.append(digit)
    while k > 0:
        k -= 1
        stack.pop()
    return "".join([str(x) for x in stack])


if __name__ == "__main__":
    _, k = input().split()
    n = input()

    k = int(k)

    print(sol(n, k))
```
