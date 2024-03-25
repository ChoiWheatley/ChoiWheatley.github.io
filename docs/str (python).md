---
aliases: 
tags: 
description:
title: str (python)
created: 2024-03-25T23:14:56
updated: 2024-03-25T23:26:52
---

## 문자열 AS 시퀀스 자료형

시퀀스란? 요소들이 연속적으로 이어진 자료형을 의미합니다. 대표적으로 리스트, 문자열, 튜플이 있습니다. (참조: [Sequence Types - list, tuple, range](https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range))

### 시퀀스 자료형의 연산들

- 인덱싱 연산자 `s[i]` : 예를 들어 `s[0]` 과 같이 변수명 뒤에 요소의 순번을 입력하여 그 위치에 해당하는 요소를 가져올 수 있습니다.
- `in` 연산자 (`x in s`): s 안에 x가 있다면 True를, 없다면 False를 반환합니다. 예: `3 in [1, 2, 3] # True`
- 더하기 연산자 `+`: 대수 연산이 아닌, 좌항의 시퀀스 뒤에 우항의 시퀀스가 연결된 새 시퀀스를 반환합니다. 예를 들어 `"hello" + "world" => "helloworld"`
- 곱하기 연산자 `*`: 시퀀스 더하기 연산을 우항의 숫자만큼 한 결과의 새 시퀀스를 반환합니다. 예를 들어 `"123" * 2 => "123123"`
- `len`: 길이를 알아내는 내장함수 예: `len("hello") => 5`
- 슬라이싱 `s[i:j]`: i번째 인덱스부터 j + 1번째 인덱스까지의 시퀀스를 반환합니다.
- 슬라이싱 `s[i:j:k]`: i번째 인덱스부터 j + 1번째 인덱스까지 k 스텝만큼 건너뛴 시퀀스를 반환합니다.
- `min(s)`: s에서 가장 작은 요소를 반환합니다. 각 문자가 숫자로 인코딩 되어있기 때문에 대소 구별이 가능합니다.
- `max(s)`: s에서 가장 큰 요소를 반환합니다.
- 음수 인덱싱: -1부터 마지막 요소로부터 시작해 -2, -3, … 인덱싱 수행시 거꾸로 요소를 찾습니다.
- `s.index(x, i, j)`: s에서 x가 나타난 바로 첫번째 인덱스를 반환합니다. 이때 i, j가 주어지면 인덱스 i부터 j + 1 사이에서 찾습니다.
- `s.count(x)`: s에서 x가 나타난 횟수를 반환합니다.

## 문자열 연산들

- `s.split(',')`: 문자열 s에서 `,`를 기준으로 단어를 분리하고 이를 각각의 리스트의 요소로 만듭니다. (참조: [`str.split`](https://docs.python.org/3/library/stdtypes.html#str.split))
- `s.strip()`: 문자열 s에서 좌측, 우측에 남아있는 공백문자 `\n`, `\t`, ` `(space) 등을 제거해줍니다. (참조:  [`str.strip`](https://docs.python.org/3/library/stdtypes.html#str.strip))

## 참고자료

- [Sequence Types - list, tuple, range](https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range)
- [`str.split`](https://docs.python.org/3/library/stdtypes.html#str.split)
- [`str.strip`](https://docs.python.org/3/library/stdtypes.html#str.strip)

## 이해도 자가확인 체크리스트

본인이 어떤 개념을 이해했는지 확인하는 가장 좋은 방법은 소리내어 말해보는 것입니다! 아래의 질문들에 대한 답변을 입을 열어 대답해 보세요!

- 아래 코드는 슬라이싱을 수행하는 코드입니다. 아래 코드를 실행시켜본 다음, 그 결과를 추론하여 아래 문제의 정답을 만들어내는 코드를 작성해 보세요!

```python
my_str = "abcdefghijklmnopqrstuvwxyz"
print(my_str[:])
print(my_str[1:])
print(my_str[:5])
print(my_str[::2])
print(my_str[::-1])

### 문제: my_str[???] => "usqom"
```

> ⚠️ 자신있게 ‘예’라고 대답할 수 있는 항목이 절반 이하라면 다시 처음부터 학습하는 것을 권장합니다.
