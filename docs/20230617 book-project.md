---
aliases: 
tags: 
description:
created: 2023-06-17T09:32:43
updated: 2023-07-11T15:21:10
title: 20230617 book-project
---
오늘 목표
- bookstore에게 이름 붙여주기
	- ESTbook
	- Allbooks
	- BookHub
- home.html 내용물
	- card로 만들자 ==> model image + pdf 다운로드
	- 그러면 book_list.html은? ==> 그대로 있음.
[[stripe]]
[[DateTimeField {auto_now_add} {auto_now} {django}]]

- 책 제목 중복되어도 올릴 수 있게 (unique 풀기)
	- handle이 같을 수 있다 ==> 핸들을 고유하게 만들려면? 넘버링 저시기 해보자! ^i33sll

- @노희연 ==> 상품모델에 커맨트 필드를 넣는 것이 아니라 별도의 모델이 있어야 한다.
	- @최승현 나도할래

- delete
	- statusCode를 하드코딩하면 되나? ^wcz9fm
		- 애초에 200은 성공코드. 지금은 200을 보내지만 DRF를 사용하면 200response라는 저시기가 있다. 
	- `delete_product`
		- statusCode를 적극적으로 활용하는 모습이 아주 보기 좋다.
	- `login_required` 데코레이터를 여러번 썼잖아. 삭제 요청을 할 때 로그인이 필요한 상황이잖아. 이걸 쓰려고 했는데, 응답이 올 때 JsonResponse로 돌아와야 하는데, 자꾸 데코레이터로 인해 의도한 바가 안 나옴. ==> delete는 페이지가 따로 없고 버튼 자체가 fetch를 하지 않는다. 

# 24일까지
[[20230624 까지 진행할 구현사항 {book-project}]]

# 다음은...
- 책 집필한 자기 분야에 대한 브리핑 / 피드백 
	- 2023-06-19T16:00:00
- 각자 구현한 저시기에 대한 브리핑 / 피드백
	- 2023-06-20T16:00:00
	- 2023-06-22T16:00:00
	- 2023-06-24T16:00:00