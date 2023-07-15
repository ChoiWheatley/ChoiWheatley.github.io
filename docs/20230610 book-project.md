---
description:
aliases: 
tags: 
created: 2023-06-10T08:05:52
updated: 2023-07-15T21:30:22
title: 20230610 book-project
---

# My

한 달도 채 안 됐는데 갑자기 강사님의 집필마감요구에 맞서 일단 대강 써놓기로 결정했다. 대충 목차별로 사람들 나눠서 분업하기로 했는데... 일단 [머리말과 저자소개 {Notion}](https://www.notion.so/a99c4bc2c25443a4be2907c0c00b8aed?pvs=4) 노션 페이지에서 확인 바란다. 나의 역할을 정리하면

- [[Data Modeling {book-project}]]
	- @김영민: database이론적인거 + modles.py의 구현 및 스키마 설정 위주로 가면 좋을거같아요
- [[Error handling {django} {book-project}]]
- [[Securities about {https} and {jwt {cookie}, {session}}]]
- 욕심을 좀 더 내자면 [[Github 사용법 {issue}, {pull request}]]

# 회의내용

- 이슈 & PR가 좋았던 이유 | [[github issue, pull request 활용하기]]
	- 정헌: 디스코드나 다른 연락수단에서 일거리 분배하는 것보다 나은 것 같더라. 깃허브 안에서 일 분배가 끝난다.
	- 정헌: 생물관련 연구소에서 사무보조로 일한 적 있는데, 퇴사하기 직전에 사람이 없어서 개발자까지 시켰는데, 다들 주변분들이 50세 정도라서 디코, 깃헙을 사용할 줄 모르고, 카톡, 문자를 통해서 주고받더라. 그땐 그게 당연한 줄 알았는데 집필프로젝트에 들어와서 깃헙 써보자 이거 써보자 하니까 바로바로 정리가 되는게 좋은 것 같다.
	- 처음 깃 사용하면서 협업 어쩌지? 했는데 디코, 카톡으로 내용을 빨리 알고 처리할 땐 카톡으로 하는 건 맞는데 협업 하는 그런거에 대해선 효율적일 것 같다. 
		- 이슈파악, 변동사항 파악 가지고 의견 다니까.
		- 카톡 가지고는 "이슈 올려놨으니 확인부탁요" 정도는 괜찮을 것 같다.
	- 영민: PR은 무조건 그냥 써야한다. WHY? 일단 현업에서 그렇게 하고 있고, 누군가는 리뷰를 해 주면서 메인은 언제나 돌아갈 수 있게 만들어야 하고, 다른 사람이 쓴 코드가 리뷰받아야만 한다. **리뷰** 브랜치 전략은 어디서 보긴 했는데, 저렇게 할 수 있을까? 하는 고민을 PR을 통해 덜어줄 수 있겠더라.
	- 이도:  PR은 존재만 알고서 사용해볼 엄두를 못 내고 있었어요. 이번 기회에 사용해보고 PR에 대한 막연함이 확 줄었습니다. 처음 보낼 때는 코멘트도 안 쓰고 보냈는데.. 커밋 메시지 컨벤션대로 작성하는 것처럼 PR도 리뷰어가 한 눈에 보기 쉽도록 작성하는게 중요할듯 합니다!

- [# bug: 익명유저가 profile에 들어가는 것을 방지 #16](https://github.com/ESTsoft-Book-Project/bookstore/issues/16)
	- login_required 데코레이터가 있다. 뒤에 url이 없어도 작동은 하는데, 넣으면 AnonymousUser일 경우 로그인 페이지로 자동으로 보내준다. 
	- 장고인증이 들어가는 모델(eg. AbstractUser)을 상속받아 사용하면 
- [[django debug]]
- 책을 쓸 때 당장 해야할 것 같다. 본인이 쓸 수 있는거 최대한 깔끔하게

- login은 GET이 아니라 POST로 해야하는 이유
	- GET으로 하면 URL에 뜨잖아. POST는 body 안에 감싸서 집어넣지.
	- https는 서드파티 SSL 보안쪽이고. 대칭키 비대칭키 기반이고. 디버깅 할 때 request.body 나오는거 원래 그런거임. 암호화된 내용이 들어가는 건 아니고. 그 바디를 암호화하는게 HTTPS. SSL 자체가 쿠키 세션키를 가지고 열어야지 안에 데이터 내용을 볼 수 있다. [[JWT]]가 바로 이 작업을 수행해주는 거임.
- [[POST와 DELETE의 차이]]

- jquery != ajax 

- PR 보내는 규칙 정하자
	- 동작하면 바로 쏜다
	- 주기적으로 회의시간에 PR 보내자.

- AJAX가 비동기통신. 웹에서 페이지 새로고침 안하고 다른 부분 안 건들이고 댓글이면 댓글창만 바뀌게 할 수 있잖아.
	- HttpResponse하면 새로고침하고 JsonResponse하면 Json이 날아가고
	- Request -> response가 있는데, json으로 요청하면 당연히 json으로 리턴해야 한다.
	- redirect 할 필요가 없으면 json으로 보내면 되고. 
	- 클라이언트는 페이지만 받는 수동적인 존재가 아니다!

- [sign-2 브랜치에 대한 PR](https://github.com/ESTsoft-Book-Project/bookstore/pull/23/files)
	- redirect 내용 추가 => `error_message` 컨텍스트 추가
	- 로그아웃 내용 처리 
	- ajax 처리는 클라이언트에서 보내는 거니까, form에서 바로 서버로 보내지 않고 이벤트리스터를 달았다. submit이라는 이벤트가 발생하는데, 이걸 서버에 요청한느게 아니라 ajax 요청을 발생시킬 수 있다. 
	- `preventDefault`라고, 이벤트도 전파가 되는걸로 알고있는데, (DOM) 그 이벤트 자체를 막아 서버로부터의 요청을 막는다.
	- `URLSearchParams`라고 객체가 있는데, GPT의 힘을 빌어 form을 작성할 때 사용.
	- `fetch`에서 상대경로를 사용하여 접근
	- `getCookies("csrftoken")`
	- 성공여부를 200으로 체크. 홈 화면으로 리다이렉트 할 수 있게
	- JsonResponse <- HttpResponse 상속인데 부가적인 기능, 명확성을 준다.
	- 
- [[github issue, pull request 활용하기]]

# Next

- 일단 책 윤곽부터 [머리말과 저자소개 {NOTION}](https://www.notion.so/a99c4bc2c25443a4be2907c0c00b8aed)
- DRF (django rest framework) | [[drf {django rest framework}]]
	- 직렬화, 역직렬화, JSON, form 대신에 serializer
	- serializer: form 대신 JSON으로.
- 상품 모델 커스텀
	- MEDIA_ROOT: 이미지 관리하는
- 결제모델 추가
- 다양한 유즈케이스 식별 및 구현
- [[20230613 book-project]] | 2023-06-10T16:00:00
