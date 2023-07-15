---
description:
aliases: 
tags: 
created: 2023-05-13T20:45:42
updated: 2023-07-15T21:33:04
title: Implementing Bitset in python
---
- [비트셋 사용한 파이썬 코드 - 백준 27447 주문은 토기입니까?](https://github.com/OrmiCodeRanger/ChoiSeunghyeon/commit/b95f48beb847fb2a962932d6f90eacdefc70aeb3)

일단 클래스 및 구현체부터 살펴보자.

```python
MAX_DAY = 1_000_000
BIT_CNT = 32
BIN_CNT = MAX_DAY // BIT_CNT


def bin_no(idx) -> int:
    return idx // BIN_CNT


def bit_no(idx) -> int:
    return idx % BIN_CNT


class Bitset:
    def __init__(self):
        self.bits = [0 for _ in range(BIN_CNT + 1)]

    def test(self, idx) -> bool:
        return self.bits[bin_no(idx)] >> bit_no(idx) & 1 == 1

    def set(self, idx):
        self.bits[bin_no(idx)] |= (1 << bit_no(idx))

    def reset(self, idx):
        self.bits[bin_no(idx)] &= ~(1 << bit_no(idx))
```

직접 비트셋을 구현한 것을 보면 알 수 있겠지만, 파이썬에선 별도의 비트셋 객체가 없다. 물론 `pip`를 사용하여 외부 패키지([BitSet](https://pypi.org/project/BitSet/), [BitVector](https://pypi.org/project/BitVector/)) 을 사용할 수 있겠지만, 이러면 코딩테스트에서 문제를 풀 수가 없다!

## WHY Bitset?

비트셋을 사용하는 이유, 특히 코딩테스트에서 사용하는 이유는 가뜩이나 메모리를 많이 잡수시는 파이썬, 메모리를 조금이라도 아끼기 위해서이다. 예를 들어 `bool` 타입 리스트를 만들어 어떤 인덱스를 방문했는지 여부를 저장하는 `visited` 객체를 만든다고 가정해보자.

```python
visited = [False for _ in range(1_000_000)]
```

뭐 이렇게 만들어서 원하는 인덱스에 방문할 때마다 `True`로 바꿔주면 좋겠다만... 엄청난 진실이 우리를 마주하고 있다.

> [Booleans are subtype of integers](https://pypi.org/project/BitVector/)

불리언 타입은 정수의 하위타입이다. 정수타입은 기본적으로 32비트일 것이므로(아마도) 오직 참과 거짓만을 표현하는 데 무려 31개의 비트를 낭비하는 것이다. 

## How Bitset?

내가 구현한 비트셋 연산은 C++에서 제공하는 [bitset](https://pypi.org/project/BitVector/)의 시그니처를 많이 가져왔다. 
- `test` ==> 주어진 인덱스가 참인지 판별
- `set` ==> 주어진 인덱스를 True로 설정
- `reset` ==> 주어진 인덱스를 False로 설정

비트연산 `&`, `|`, `~`에 대한 설명은 굳이 하지 않겠다. 다만 헬퍼함수인 `bin_no`와 `bit_no`는 알아둘 필요가 있다.
- `bin_no`
	- `int`배열로 구성된 비트셋에서 정확한 위치의 빈을 아는 것이 중요하다. 그래야 참조가 가능하니까. 기본적으로 하나의 원소마다 총 32개의 비트를 저장하고 있기 때문에 내가 원하는 인덱스를 32로 나눈 몫이 바로 빈 번호가 된다.
- `bit_no`
	- 빈 번호를 알았으면 이제 비트 번호를 알아야 한다. 이는 간편하게 나머지 연산으로 수행이 가능하다.
