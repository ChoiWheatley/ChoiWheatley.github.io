---
description:
aliases: 
tags: 
created: 2023-05-02T09:08:31
updated: 2023-07-15T21:30:22
title: 20230502 estsoft - python - str,  cpython, indexing and slicing, numeric operations, bit operations, is, not, in
---
- [멘토님의 실시간 노트북](https://colab.research.google.com/drive/1IRa8nYwM2HtkzlNJGBlavWK96kffytm_?usp=sharing#scrollTo=E1nDm4EOyOxg)
- [별로 안 적은 구글 콜랩](https://colab.research.google.com/drive/1MLH1EUy3nmKQXPMjn5K5-FStLzh5HV5J?usp=sharing)

# str

- `lower`, `upper` 
- `find`, `index` => 모두 인자로 온 문자열 값과 일치하는 첫번째 인덱스를 리턴. 두 메서드의 차이점이라면 `find`는 일치하지 않는다면 -1을 리턴하고 `index`는 `ValueError` 에러를 발생시킨다.
- `count` => non-overlapping occurences of substring의 개수를 리턴한다고 한다. 그러니까 예를 들어 `'aaaaa'.count('aa')`의 결과로 4가 아니라 2가 나온다는 소리임.
- `strip` => 공백제거. 번외로 `rstrip`과 `lstrip`이라는 각각 오른쪽, 왼쪽 공백만을 제거하는 녀석들도 있음.
- `replace(self, old, new, count=-1)` => old와 일치하는 녀석들을 new로 바꿔준다. count가 기본 -1로 되어있는데, **모두**라는 뜻이다. 0 이상인 수로 할당해주면 그만한 개수만 수행한다.
- 메서드 체이닝 => str의 메서드들은 모두 str을 리턴하기 때문에 다음 줄에서 다시 쓸 필요 없이 그냥 계속 이어붙이기만 하면 된다.
- `split(self, sep=None, maxsplit=-1)` => 문자열을 쪼개준다. 
	- `sep`을 지정하여 특정한 구분자를 정의할 수 있다. `sep`을 지정하지 않으면 whitespace가 지정된다.
	- `maxsplit`은 이름 그대로 분할의 개수에 상한을 둘 수 있다는 것이다. 가장 왼편에서부터 시작한다.
- list comprehension

	```python
	num = "12,3,1423,53,43,556,2,1"
	numbered_list = [int(i) for i in num.split(',')]
	```

- `map` 잠깐 핥아가셨네 사실 list comprehension은 아래처럼 쓸 수도 있다.

	```python
	num = "12,3,1423,53,43,556,2,1"
	numbered_list = list(map(int, num.split(','))
	```

- `join(self, iterable)` =>자기자신을 구분자로 `iterable`을 순회하며 하나의 문자열로 만든다.
	- `'.'.join(['ab', 'pq', 'rs']) -> 'ab.pq.rs'`

```python
# 퀴즈
# 숫자만 모두 더하라!
num = "123abc913sldkf"
result = sum(map(int, filter(lambda x: x.isnumeric(), num)))
print(result)
```

- `rjust(self, width, fillchar=' ')` => 오른쪽 정렬
- `ljust(self, width, fillchar=' ')` => 왼쪽 정렬
- `center(self, width, fillchar=' ')` => 가운데 정렬
- `maketranse(self, ...)` => `str.translate(self, table)`에 필요한 `table`을 만드는데, 얘는 `replace`이긴 하지만 table에 있는 다양한 녀석들을 매핑하여 저시기 할 수 있다.

# indexing and slicing

슬라이스 결과는 `slice` 객체이다. 내부적으로 `slice()` 내장함수를 호출한다.

```python
a[start:stop]  # items start through stop-1
a[start:]      # items start through the rest of the array
a[:stop]       # items from the beginning through stop-1
a[:]           # a copy of the whole array

a[start:stop:step] # start through not past stop, by step

# start and stop may be a negative number
a[-1]    # last item in the array
a[-2:]   # last two items in the array
a[:-2]   # everything except the last two items

# steps can be a negative number
a[::-1]    # all items in the array, reversed
a[1::-1]   # the first two items, reversed
a[:-3:-1]  # the last two items, reversed
a[-3::-1]  # everything except the last two items, reversed
```

# numeric operation

```python
a = 10
b = 3

print(f'10 + 3 == {a + b}')
print(f'10 - 3 == {a - b}')
print(f'10 / 3 == {a / b}')
print(f'10 // 3 == {a // b}') # 몫만 나옵니다.(정수만요!)
print(f'10 * 3 == {a * b}')
print(f'10 ** 3 == {a ** b}')
print(f'10 % 3 == {a % b}') # 나머지
```

# is, not

`is`는 ` == ` 과 다르게 객체의 `id`를 보고 비교한다. 그래서 다음과 같은 이상한 현상을 발견할 수 있다.

```python
a = 256
b = 256
assert a is b
```

-5부터 256까지는 기본 공간에 저장되어있는 수를 가리킨다고 했지?

```python
a = 9999
b = 9999
assert a is not b
```

리스트는 언제나 항상 다르다.

```python
a = [1, 2, 3]
b = [1, 2, 3]
assert a == b
assert a is not b
```

# member operator in

```python
assert not 'a' in 'hello, world'
assert 'a' in ['a', 'b']
assert 10 in {'a':10, 'b':20}.values()
```
