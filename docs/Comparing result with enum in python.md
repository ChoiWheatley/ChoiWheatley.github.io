---
description:
aliases: 
tags: 
created: 2023-05-13T21:18:06
updated: 2023-07-15T21:30:21
title: Comparing result with enum in python
---

# Comparing result with enum in python

- [enum 사용한 파이썬 코드 - 백준 27447 주문은 토기입니까?](https://github.com/OrmiCodeRanger/ChoiSeunghyeon/commit/b95f48beb847fb2a962932d6f90eacdefc70aeb3) 

```python
from enum import Enum, auto

class Cmp(Enum):
	Less = auto()
	Greater = auto()
	Equal = auto()

def compare(lhs, rhs) -> Cmp:
	if lhs < rhs:
		return Cmp.Less
	if rhs < lhs:
		return Cmp.Greater
	return Cmp.Equal
```

러스트에서 [다음과 같은 코드](https://users.rust-lang.org/t/greater-than-less-than-in-a-match-block/63399/5)를 본 나는 파이썬에도 똑같이 적용할 수 있을 것 같았다. 일단 파이썬에는 당연하겠지만 `Compare` enum은 존재하지 않을 것 같아 직접 만들었다. `auto`는 굳이 구체적인 숫자를 붙이기 싫을 때 사용하는거다.

중요한 건 `compare`이다. 이 함수가 두 객체를 직접 비교하여 `Cmp` 오브젝트를 리턴하는 역할을 가지고 있다.

## But... Why?

백준 27447 문제를 풀면서 이건 `match`로 풀면 딱이겠다고 생각하며 발상한거라서 해당 문제풀이 코드 일부를 아래에 기술한다.

```python
for day in range(last_time + 1):
	delta_time = 1 << 31

	if len(schedule) > 0:
		delta_time = schedule[0] - day

	match (bitset.test(day),        # does customer come?
		   compare(delta_time, M),  # is it right time to brew a coffee?
		   bowl_cnt > 0,            # is empty bowl remains?
		   coffee_cnt > 0):         # is coffee ready?

		case (True, _, _, True):
			# 사람이 와 있고 커피가 준비가 돼 있으면 서빙한다.
			coffee_cnt -= 1

		case (True, _, _, False):
			# Customer says, no coffee?? It's terrible!!!
			return False

		case (False, Cmp.Greater, _, _):
			# 사람이 아직 안 와 있고, 커피 우릴 시간이 아직 안 돼 있으면 일단
			# 그릇부터 만들어 놓는다.
			bowl_cnt += 1

		case (False, Cmp.Less | Cmp.Equal, False, _):
			# 사람이 아직 안 와 있고, 커피 우릴 시간이 적당한데 그릇이 없으면
			# 일단 그릇부터 만들어 놓는다.
			bowl_cnt += 1

		case (False, Cmp.Less | Cmp.Equal, True, _):
			# 커피를 우린다. (그릇을 커피로 변환한다)
			coffee_cnt += 1
			bowl_cnt -= 1
			schedule.popleft()

		case _:
			raise RuntimeError('Unknown arm has detected!')
```

`compare` 함수 덕분에 `delta_time`과 `M` 사이의 관계를 조금 더 유연하고 좋은 가독성으로 표현할 수 있게 되었다. 만약에 `match` 구문이나 `Compare` 없었다면 `if`문으로 가득 도배된 코드를 보게 되었을 것이다. 

내가 `match`구문을 좋아하는 이유가 바로 여기에 있다. `match` 관련 문서에서 자세히 다루겠지만 일단 다중 `if`문의 지옥으로부터 빠져나올 수 있다는 것 만으로 충분히 가치가 있다.
