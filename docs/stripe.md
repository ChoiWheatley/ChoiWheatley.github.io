---
aliases: 
tags: 
description:
created: 2023-06-17T11:29:45
updated: 2023-07-11T15:21:07
title: stripe
---
- 결제 모듈
- 회원가입 후 stripe key 발급, `.env` 파일에 추가
- 모든 작업은 `purchases`에서 진행한다.
- 모든 `Product`는 유일하므로 `Purchase`와는 1:1 관계이다.
- [?] `stripe_price_id`의 용도는 문서를 통해 봐야겠다.
- 순서
	1. 