---
aliases: 
tags: 
description:
created: 2023-06-20T09:37:03
updated: 2024-11-30T17:44:18
title: ManyToManyField {django}
---
- ref: 
	- <https://docs.djangoproject.com/en/5.1/ref/models/fields/#manytomanyfield>
	- <https://docs.djangoproject.com/en/5.1/topics/db/models/#many-to-many-relationships>
	- <https://docs.djangoproject.com/en/5.1/topics/db/examples/many_to_many/>

Django는 ManyToManyField가 정의되면 자동으로 중계 테이블을 생성해준다. 테이블 이름은 `<table_a>_<table_b>_<hash>` 의 형태가 된다. 임의의 중계테이블 이름을 정의하고 싶은 경우, `db_table` 키워드 인자를 적용해주면 된다.

만약 중계테이블에 추가적인 메타데이터를 필요로 하는 경우, `through` 키워드 인자를 사용하여 원하는 모델을 중계테이블로써 활용도 가능하다. [Extra fields on many to many relationships](https://docs.djangoproject.com/en/5.1/topics/db/models/#extra-fields-on-many-to-many-relationships)

> [!question] M2M 필드를 갖고있는 모델이 약한개체인건가? 

> Generally, ManyToManyField instances should go in the object that's going to be edited on a form.  
> 일반적으로, ManyToManyField 인스턴스는 **직접적으로 사용될** 객체에 정의되는 것이 올바릅니다.

예를 들어 "피자에 올릴 토핑"이 "토핑이 올라갈 피자" 보다는 상식적이기 때문에 Pizza에 ManyToManyField를 정의하는 것이 올바릅니다.

---

## field 추가

M2M 필드의 `add` 메서드를 호출하거나 `create` 메서드를 호출하면 된다.

## 쿼리

M2M 필드의 `all`, `get`, `filter`를 모두 사용 가능하다. [[querying in {django query}]]
