---
description:
aliases: 
tags: 
created: 2023-05-17T23:44:09
updated: 2023-07-15T21:33:03
title: sort - sorted - key - index 추적 - python
---

# Sort

- `sort`는 원본을 만지고, 반환값이 없음 (`None`을 리턴함)
- `sorted`는 원본을 만지지 않고 정렬된 새 리스트를 반환함
	- `key` 인자는 비교를 수행할 말 그대로 키 값을 리턴하는 함수를 받는다. 이를 이용해 커스텀 정렬을 수행할 수 있게 된다.
- 공통: 옵션 중에 알아두면 좋을 `reverse`는 기본 `False`로 설정되어 있지만 `True`로 설정될 시 descending order로 정렬을 수행한다.

```python
# Practice 1
l = [[1, 10, 'leehojun'], 
	 [20, 30, 'hojun'], 
	 [10, 20, 'weniv'], 
	 [1, 2, 'likelion'], 
	 [55, 11, 'sun']]

print('1. 글자 수 대로 정렬해주세요')
print(sorted(l, key=lambda x: len(x[2])))
print()
```

만약 리스트 정렬 수행결과 뿐만 아니라 옮겨진 **인덱스**까지 추적하고 싶다면 [다음 토론](https://stackoverflow.com/questions/7851078/how-to-return-index-of-a-sorted-list#7851166)을 보는 것이 좋다. pandas의 `sorted_index`를 사용하는 것보다 이게 훨씬 낫다!!!

```python
l = [10, 20, 30, 2, 1, 8, 13, 17, 5, 20]
print(f'sorted list: {sorted(l)}')
print(f'sorted index: {sorted(range(len(l)), key=lambda k: l[k])}')
```

`filter`를 사용하면 `remove`를 여러 번 사용한 효과를 볼 수 있다.

```python
list(filter(lambda x: x != 20, [10, 20, 30, 40, 50, 20, 20, 20]))
```

## 갑분 좌표평면 차원축소 문제와  zip 활용법

https://codingdojang.com/scode/408?answer_mode=hid  
좌표평면 문제가 나오면 차원 축소나 차원 확대가 가능한 문제인지 확인할 것.

> 1차원의 점들이 주어졌을 때, 그 중 가장 거리가 짧은 것의 쌍을 출력하는 함수를 작성하시오. (단 점들의 배열은 모두 정렬되어있다고 가정한다.)

> 예를들어 S={1, 3, 4, 8, 13, 17, 20} 이 주어졌다면, 결과값은 (3, 4)가 될 것이다.

`zip`을 활용하면 두 iterable들을 쌍쌍바처럼 묶어준다. 아래 코드의 `zip(S, S[:1])`의 의미는 S의 모든 원소와 S의 첫번째 원소부터 시작하는 저시기를 묶어준다. 범위 초과에 대하여 알아서 처리하는 걸 보니 매우 똑똑한 녀석인듯.

```python
S = [1,3,4,8,13,17,20]
print('1차: zip으로 쌍쌍바 묶기')
zipped = zip(S, S[1:])
print(sorted(zipped, key=lambda z: z[1] - z[0])[0])
```

```python
help(list.sort)
print("#####################")
help(sorted)

# Practice 1
l = [
    [1, 10, "leehojun"],
    [20, 30, "hojun"],
    [10, 20, "weniv"],
    [1, 2, "likelion"],
    [55, 11, "sun"],
]

print("1. 글자 수 대로 정렬해주세요")
print(sorted(l, key=lambda x: len(x[2])))
print()

print("2. 맨 앞에 위치한 숫자대로 정렬해주세요.")
print(sorted(l, key=lambda x: x[0]))
print()

print("3. 중앙에 위치한 값대로 정렬해주세요.")
print(sorted(l, key=lambda x: x[1]))
print()

# Practice 2
l = [[1, 10, 32], [20, 30, 11], [10, 20, 22], [1, 2, 13], [55, 11, 44]]
print("4. 3개의 전체 합이 작은 순서대로 출력해주세요.")
print(sorted(l, key=sum))
print()
```

# 인덱스 추적

만약 리스트 정렬 수행결과 뿐만 아니라 옮겨진 **인덱스**까지 추적하고 싶다면 [다음 토론](https://stackoverflow.com/questions/7851078/how-to-return-index-of-a-sorted-list#7851166)을 보는 것이 좋다.

pandas의 `sorted_index`를 사용하는 것보다 이게 훨씬 낫다!!!

```python
l = [10, 20, 30, 2, 1, 8, 13, 17, 5, 20]
print(f"sorted list: {sorted(l)}")
print(f"sorted index: {sorted(range(len(l)), key=lambda k: l[k])}")
```
