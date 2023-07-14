---
aliases: 
tags: 
description:
created: 2023-06-20T09:37:03
updated: 2023-07-11T15:21:08
title: ManyToManyField {django}
---
- [docs.com](https://docs.djangoproject.com/en/4.2/topics/db/examples/many_to_many/)

나는 지금까지 M2M 관계에 대해선 *무조건*  테이블을 하나 더 파야 된다고 생각했는데 장고에선 그럴 필요가 없게 만들어줬네

- [?] M2M 필드를 갖고있는 모델이 약한개체인건가? 

- field 추가
	- M2M 필드의 `add` 메서드를 호출하거나 `create` 메서드를 호출하면 된다.
- 쿼리
	- M2M 필드의 `all`, `get`, `filter`를 모두 사용 가능하다. [[querying in {django query}]]

# in book-project...

우리 프로젝트에서는 User가 여러권의 책을 구매할 수 있고, 또 책 한 권은 여러 User가 구매할 수 있다. 하지만 우리는 그 내역을 `Purchase`라는 모델로 관리하고 있고, 따라서 우리는 M2M 필드를 사용하기 보다는 세 개의 테이블을 활용한 `ManyToOne` 필드를 사용하는 방법밖에 없겠다. 

![[Drawing 2023-06-20 10.36.43.excalidraw]]

