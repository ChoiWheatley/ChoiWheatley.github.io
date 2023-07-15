---
aliases: 
tags: 
description:
created: 2023-06-28T14:01:49
updated: 2023-07-15T21:30:21
title: 20230628 book-project
---
[[EC2 Postgresql을 장고 기본 데이터베이스로 활용하기]] 마저 진행하기

# TODO

![[20230627 book-project#^u6tdmc]]

## updatepassword

[[drf {django rest framework}]] 를 공부하면서 구현하는 비밀번호 변경 로직

Serializer를 사용해서 수정하는 건 된다. 문제는 signout으로 리다이렉트가 안된다. js 단에서 `window.location.href`를 하는 것 가지고선 요청이 날아가지 않는다는 것을 알았다. 그러면 이번에도 fetch를 해야하나? 애초에 redirection 자체가 아닌걸.

@김영민 message랑 redirect url을 왜 다 백엔드에서 보내냐?진짜 필요할 때에만 보내야지, 사실은 status code만 오가도 동작하게 만들어야 한다.

https://realpython.com/django-rest-framework-quick-start/ 다음 튜토리얼을 봐봐. view 함수들이 죄다 간결하게 `serializer.data` 또는 `serializer.errors` 또는 `status`만을 실어서 보내고 있잖아.

[[fetch api error handling {js}]]

아, validator들을 어떻게 실패시켜야 할지 전혀 감이 오지 않아..
