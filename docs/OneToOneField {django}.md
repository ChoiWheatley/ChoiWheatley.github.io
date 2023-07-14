---
aliases: 
tags: 
description:
created: 2023-06-20T11:15:22
updated: 2023-07-11T15:21:07
title: OneToOneField {django}
---
- [doc](https://docs.djangoproject.com/en/4.2/topics/db/examples/one_to_one/)
- [?] [[ForeignKey {django}]]와의 차이점이 뭐냐? 나는 ForeignKey가 곧 1:1 관계인 줄로만 알았는데?
	- `ForeignKey`는 0가 가능하고, `OneToOneField`는 엔트리 생성시 반드시 명시해야한다.
	```python
		restaurant = Restaurant(
			place=Place(name="승현이네", address="원인재로 14"), 
			serves_hot_dogs=True, 
			serves_pizza=False)

		restaurant.place # 승현이네
	```
	- 물론 반대방향으로는 1:1 관계를 만들 의무는 없기 때문에 `ObjectDoesNotExist` 에러가 발생할 여지가 있다는거. → `hasattr` 를 사용하여 에러처리 가능
	 ```python
		place2 = Place(name="정현이네", address="원인재로 15")
		place2.restaurant # raises `ObjectDoesNotExist`
	```