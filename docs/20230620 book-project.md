---
aliases: 
tags: 
description:
created: 2023-06-20T16:03:07
updated: 2023-07-11T15:21:10
title: 20230620 book-project
---
- [[inventory modeling {book-project}{재고, 장바구니, 구매링크}]]
- [[querying in {django query}]]
- [[image(media) 파일을 DB 이외의 로컬 저장소에서 관리하게 만들기 {django}]] @김영민 ^qwymrr
	- base64를 사용했군
	- `ContentFile`이라는 파일관리의 장고 측면.
	- `MEDIA_ROOT`과 static을 url에 추가했다고? https://github.com/ESTsoft-Book-Project/bookstore/pull/44/files#diff-5281585d6c91f7b61204815dc902702dfb0cb369a99aabd9a26a84ab37194f1bR29
	- `ImageField(upload_to='product_images/')` 를 사용하여 데이터베이스가 아니라 로컬 파일 시스템 안에 이미지를 관리하고 있다!!! 이거 중요하다. 나중에라도 [[Resource Storage와 Database를 연동하는 방법에 대하여 알아봅니다.]] 에서 확인
- 결제모듈
	- 페이코 → 카페로 바꿀 예정
	- 카카오페이
	- 네이버페이 → 카페로 바꿀 예정
- OAuth
	- 네이버 로그인 @임동성
	- 구글 로그인 @신태우
- 장바구니 @전정헌 @김태은
	- 장바구니 담는 기능, 리스트 출력하기
	- 엄청많이 하셨네

- [[How to manage static files (e.g. images, js, css) {django}]]