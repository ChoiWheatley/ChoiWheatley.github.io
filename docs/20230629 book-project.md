---
aliases: 
tags: 
description:
created: 2023-06-29T07:13:37
updated: 2023-07-15T21:30:21
title: 20230629 book-project
---
- [[0014.1.1 drf {django rest framework} 😴]] Serializer에 대하여 먼저 공부.
- [[REST error message in HTTP Header or Response Body {sof}]]
- `logout` 메서드를 안 쓰고 `delete_user`에 리디렉션을 `window.location.href`로 보냈더니 로그아웃을 안 하던데?
- [[getattr, setattr {python}]]
- [[drf 에서 redirect url에 대한 정보를 ViewSet에 담는법 {drf}]]
- [[업무로직을 프론트에서 짜야합니까, 백엔드에서 짜야합니까]]
- [[response.headers.get(Content-Type) {js}]]

# [[주니어 백엔드 개발자, 그 이상으로 🚀{book-project}|집필]] 피드백

- 전체적으로 좀 안보인다. 너무 빽빽함
	- 이게뭘 말하고 하는건지 바로 
	- 단계가 너무 깊음
	- 목차 구분자를 `-`으로 통일, OAuth 서식으로 통일하자
- 코드 스크린샷 대신에 코드블럭으로 바꿀것
- 한 페이지가 비는 것들이 많다.
- [ ] 에러처리 관련 내용 채우자

# 구현 피드백

- cart
	- [ ] 체크가 0개일때 주문하기 버튼을 비활성화
	- [ ] 카트상품이 0개일때 JS null 에러발생
- password validation을 사용하여 signup 로직도 변경하기
