---
aliases: 장바구니, 재고
tags: 
description:
created: 2023-06-20T14:49:30
updated: 2023-07-15T21:33:04
title: inventory modeling {book-project}{재고, 장바구니, 구매링크}
---

# linked issue

- [Cart's Goal](https://github.com/ESTsoft-Book-Project/bookstore/issues/72)
- [Delete Cart elements on purchase success](https://github.com/ESTsoft-Book-Project/bookstore/issues/105)
---
- `Product.stock` 필드를 추가하여 재고관리에 관한 로직을 추가한다.
	- [v] 책 상품 생성 시 초기 재고를 설정해야함. 설정 안하면 기본 0
	- [v] 책 상품 수정 시 재고를 변경하는 유즈케이스.
- 한 번의 Purchase에 여러 Product를 구매하는 유즈케이스를 추가한다.
	- [x] 구매링크를 눌렀을 때 바로 결제창으로 넘어가는 것이 아니라 남은 재고의 수량, 결제모듈 선택하는 중간단계 필요 → 주문관리 [[#^rgsliy]]
		- [ ] POST 요청을 통한 Cart DB 상태변경 @최승현
	- [x] 장바구니와는 다르다! 하지만 여러 Product에 대한 종합 결제대금을 구하는 로직이 필요하겠군. [[#^rgsliy]]
	- 장바구니, 주문관리 팀과 함께 작업을 진행해야겠다. ![[20230624 까지 진행할 구현사항 {book-project}#^xjs0fh]]
- [ ] stock == 0인 제품들은 book_list에 보이지 않는 유즈케이스
- [v] 수량을 장바구니에서도 조절하는 유즈케이스 (proposal)
- 참고: [[purchase history]] 는 다른 개념이다. proposal에 넣어두면 좋겠다.

# 장바구니팀 + 재고팀 목표

- @최승현
	- [v] inventory 브랜치를 구현 및 머지 후 장바구니 팀으로
	- [ ] a 태그 "주문하기"에 POST 요청 추가하기
- @전정헌 @김태은
	- 장바구니 마무리 
		- 수량조절, 장바구니 안에서 숫자 바꾸기 + 삭제
			- [v] 재고를 초과한 경우 alert 띄우기
		- '장바구니 담기' 눌렀을 때 장바구니로 이동 OR 상세창에서 남아있기
		- [v] 장바구니 체크박스 → 총 주문금액 보기 → [987fb69](https://github.com/ESTsoft-Book-Project/bookstore/blob/987fb6928e2625706a97749c2cc032c4992440c0/templates/cart_list.html#L98-L107)
		- [v] 장바구니 내에서 이미지 보여주기 `getImageUrl`
		- [v] 상품명 자리에 `a` 태그를 사용, 다시 상품 상세페이지로 이동
	- 주문관리창 @김태은 ^rgsliy
		- 템플릿 파일
			- [ ] 주문페이지에 맞게 수정 필요
		- [ ] 장바구니에서 체크한 제품들만 주문창에 올라오기
		- [x] 주문자 정보(User 정보)
		- 결제상세
			- [x] 총 주문금액
		- 결제 수단
			- [x] stripe
			- [x] 카페
		- 결제하기 버튼
		- order cancle 버튼 @김영민 문의
