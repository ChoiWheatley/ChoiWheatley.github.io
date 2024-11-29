---
aliases: 
tags: 
description:
created: 2023-07-06T10:10:41
updated: 2023-07-15T21:33:05
title: F(), Q() in {django query}
---

# `F()`

https://docs.djangoproject.com/en/4.2/ref/models/expressions/#f-expressions

> F() 개체는 모델 필드의 값, 모델 필드의 변환된 값 또는 주석이 달린 열을 나타냅니다. 이를 통해 모델 필드 값을 참조하고 이를 사용하여 데이터베이스 작업을 수행할 수 있으므로 실제로 데이터베이스에서 파이썬 메모리로 가져올 필요 없이 데이터베이스 작업을 수행할 수 있습니다.

ORM을 사용할 때 파이썬이 아니라 DB 자체 쿼리를 사용하게 만들어주는 메서드. 

```python
Post.objects.annotate(title=F("post__comment")).values("title")
```

## high performance

벌크 업데이트를 수행할 때 파이썬으로 가져올 필요가 없으므로 성능이 높다.

```python
Reporter.objects.update(stories_filed=F("stories_filed") + 1)
```

## race conditions

또, DB가 필드 값을 업데이트 하므로 race condition이 일어나지 않는다는 점도 장점이라고

## returns PK rather than model instance

```python
Company.objects.annotate(built_by=F("manufacturer"))[0]
```

# `Q()` 

- https://docs.djangoproject.com/en/4.2/topics/db/queries/#complex-lookups-with-q-objects

AND(`&`), OR(`|`), NOT(`~`), XOR(`^`) 연산을 좀 더 편하게 만들어준다.

```python
Poll.objects.get(
    Q(question__startswith="Who"),
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6)),
)
```
