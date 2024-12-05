---
aliases: 
tags: 
description:
title: 필터링 알고리즘을 설계하면서 {django orm}
created: 2024-12-05T16:37:51
updated: 2024-12-05T17:23:10
---
이 문서는 PlanA ("플라나" 라고 읽음) 프로젝트를 진행하며 만난 필터링 알고리즘에 대해서 작성한 글이다.

## 문제발견

메모와 스케줄, 메모와 투두리스트 간에 서로 연관관계가 있을 수 있는 엔티티 "Memo" 리스트를 쿼리하기 위해 쿼리 파라메터를 사용하여 필요한 메모만 필터링하는 로직을 추가하기로 결정했다. `type[]` 이라는 쿼리 파라메터이고, ([[Query Parameter가 List, Dict 타입일 경우]]에 대응한 키 이름임) 리스트를 입력으로 받아 각 원소들이 명시한 연관관계들만을 포함하여 필터링하기로 결정했다. 예를 들어,

- `?type[]=schedule` ⇒ schedule과 연관관계가 있는 메모만 쿼리
- `?type[]=schedule&type[]=todo` ⇒ schedule과 todo 연관이 있는 메모만 쿼리
- `?type[]=` ⇒ schdeule, todo 둘 다 연관이 없는 솔로 메모만 쿼리 **여기가 맛도리**
- `?type[]=&type[]=schedule` ⇒schedule과 연관이 있거나 솔로 메모만 쿼리

이런 식으로 결과가 반환되길 원했던 것이다. [[F as field, Q as query in {django query}]] 문서에 따른 Q 오브젝트를 활용하여 반복문을 돌며 `OR` 연산을 체이닝 하면 될 것이라고 생각한 나는 금세 막다른 길에 다다르고 말았다. 바로 솔로 메모를 포함시키는 방법에 대해서였다.

[Django Field Lookups](https://docs.djangoproject.com/en/5.1/ref/models/querysets/#field-lookups) 에서 `isnull` 을 확인하고, 메모와 스케줄, 메모와 투두 간에 연관관계가 null인지 여부를 판단하면 되는 단순한 과정이었기 때문에 나는 위 예시의 처음 두 사례는 아래와 같이 적기만 하면 되었다:

```python
q = Q()
for t in types:
	match t:
		case "schedule":
			q |= Q(memo_schedule__isnull=False)
		case "todo":
			q |= Q(memo_todo__isnull=False)
```

> 지금 와서 보니 이 로직에도 문제가 있었다! OR 연산이기 때문에 schedule만 쿼리하고 싶어도 todo관계가 얽힌 메모가 같이 딸려온다.

하지만 문제는 공백 문자열, 즉 솔로 메모인 케이스가 문제였다. 솔로 메모가 포함되는 순간 필터링 로직이 꼬여버렸기 때문이다(적어도 내 머리에서는). 세번째 예시인 `?type[]=` 만 봤을땐 단순히

```python
q &= Q(memo_schedule__isnull=True) & Q(memo_stodo__isnull=True)
```

이렇게 작성하면 끝날 줄 알았고 이 케이스**만** 통과할 수는 있다. 하지만 네번째 케이스처럼 솔로 메모와 다른 엔티티와의 관계가 섞여버리는 순간 💣. 이미 AND 연산으로 모든 연관관계를 막아버렸는데 다른 연관관계를 아무리 애써 추가하려해도 걸러져 버릴 것이 아닌가? 이것은 마치 `True AND False = False` 로직과도 같은 결과다.

## Reverse Logic

그래서 안쓰는 연관관계들만 Exclude 하는 방식을 떠올렸다. 그렇다면 위의 예제들은 아래와 같이 다시 작성할 수 있게된다:

- `?type[]=schedule` ⇒ exclude todo and ""
- `?type[]=schedule&type[]=todo` ⇒ exclude ""
- `?type[]=` ⇒ exclude schedule and todo
- `?type[]=&type[]=schedule` ⇒ exclude todo

exclude 하면 좋은 점: AND 연산을 연쇄해도 된다! 어차피 걸러질 놈들은 `??? AND False` 해서 두 번 다시 포함되지 않게 만들면 될 뿐이다. 그래서 schedule, todo 연관관계는 아래와 같이 작성할 수 있다:

```python
q = Q()
for e in excluded_types:
	# 여기서 e는 걸러질 타입을 의미한다!
	match e:
		case "todo":
			q &= Q(memo_todo__isnull=True)
		case "schedule":
			q &= Q(memo_schedule__isnull=True)
```

그렇다면 "" 타입은? ""를 exclude한다는 뜻은 적어도 하나 이상의 연관관계를 맺은 메모이기만 하면 상관이 없다. 따라서 아래와 같이 작성하면 된다.

```python
q &= Q(memo_schedule__isnull=False) | Q(memo_todo__isnull=False)
```

연관관계를 포함하는 두 Q 객체를 OR로 묶었고 그것을 AND로 감쌌기 때문에 부울식을 풀때 분배법칙이 성립한다. 예를 들어, `F & (T | F) = (F & T) | (F & F) = F` 로 볼 수 있다. 따라서, "" 타입을 exclude 해도 다른 엔티티를 exclude하면 분배법칙 때문에 그 엔티티는 연관관계에서 제외될 수 있는 것이다!

아래 필터링 로직 전문을 추가한다:

```python
# `type` filtering
if param.get("type[]") is not None:
	types = set(param.getlist("type[]"))
	excluded_types = {"todo", "schedule", ""} - types

	q = Q()
	for e in excluded_types:
		# 여기서 e는 걸러질 타입을 의미한다!
		match e:
			case "todo":
				q &= Q(memo_todo__isnull=True)
			case "schedule":
				q &= Q(memo_schedule__isnull=True)
			case "":
				q &= Q(memo_todo__isnull=False) | Q(memo_schedule__isnull=False)

	queryset = queryset.filter(q)
```
