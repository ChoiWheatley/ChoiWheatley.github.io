---
aliases: 
tags: 
description:
created: 2023-06-19T13:25:48
updated: 2023-07-11T15:21:10
title: 20230624 까지 진행할 구현사항 {book-project}
---

- DRF {users, products}
	- @전정헌 @임동성, @최승현(깍두기)
- 장바구니, 주문 관리 ^xjs0fh
	- @전정헌 @김태은
- 리뷰(comment), 이미지 관리
	- @노희연 @김영민
- 재고 관리 | [[inventory modeling {book-project}{재고, 장바구니, 구매링크}]]
	- @최승현
	- Product (one to many) Purchase (many to one) User
- OAuth
	- @김이도
- 공부
	- @김태은
- 결제관리
	- @김영민
	- 카카오페이
		- @송주헌
	- 페이코
		- @신태우
	- 몰라도 된다. 그냥 가져다 써도 된다. 책 굳이 안 써도 된다.
		- [!] Stub 로직을 책에 작성한다. stripe, 카카오페이 같은 건 구현은 할 거지만 책에 작성하지 않는다.
	- 재고가 1 줄어드는 작업은 다른 사람이 해야 한다.
- Proposal
	- [[purchase history]]
