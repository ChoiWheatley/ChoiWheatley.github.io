---
description:
aliases: 멱등 REST
tags: SOF
created: 2023-06-08T16:23:59
updated: 2023-07-15T21:33:03
title: "PUT is idempotent, PATCH isn't"
---
- [[PATCH {HTTP}]]
- [Use of PUT vs PATCH methods in REST API real life scenarios {SOF}](https://stackoverflow.com/questions/28459418/use-of-put-vs-patch-methods-in-rest-api-real-life-scenarios/39338329#39338329)
- What is idempotent? 멱등성이란?
	- 수학용어에서 온 멱등성은 모든 $x$에 대하여 $f(f(x)) = f(x)$ 를 만족하는 함수 $f$를 멱등함수라고 부른다. 그러니까 자연수를 예로 들면 곱하기 1 같은건 몇번을 x에 적용해도 계속 같은 값이 나오잖아. 
- PUT이 멱등인 이유
	- PUT은 멱등이다. 유저 필드를 변경하기 때문이다. 만약 같은 필드로 여러번 PUT 요청을 보낸다면 당연하게도 계속 같은 값으로 변경될 것이다.
	- 예를 들어 `/users/<id>` URI에게 다음과 같은 PUT 요청을 보낸다고 가정하자.

	```
	PUT /users/1
	[{"username": "asdf", "email": "test@example.com"}]
	```

	- 해당 요청을 수백 번 보낸다고 할지라도 우리가 `id:1` 유저의 상태, 데이터베이스 전체 상태가 변하지 않는다는 사실은 잘 알 수 있다.
- PATCH가 멱등이 아닌 이유
	- PATCH는 겉으로 보기엔 단지 엔티티의 필드 일부를 변경하는 것처럼 보인다. 하지만 그것은 틀렸습니다. [다음 답변](https://stackoverflow.com/a/39338329)을 보면 PATCH 메서드 인자의 필드에 `op: add`를 넣어 `/users` 리소스에 별도의 명령을 주입하는 것을 볼 수 있다. 따라서 같은 메서드를 여러번 호출하게 되면 이전과 다른 상태를 가지게 될 것이고, 이는 멱등규칙을 어기게 된다.
	- 물론 이 방식은 완전히 RESTful 하지 않지만 (`op`의 의도를 파악할 수 없음) [[RESTful API란 무엇인가 -- web, resources, osi 7layers]] 내용을 따르면 충분히 REST한 api를 설계할 수 있음을 알 수 있다.
