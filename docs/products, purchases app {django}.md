---
description:
aliases: 
tags: 
created: 2023-06-13T17:09:16
updated: 2023-07-11T15:21:07
title: products, purchases app {django}
---
- products app
	- Product Model
		- name
		- handle(slug)
		- price
		- user -> 외래키로 연결
		- 등등
		- 이 이하에 각종 결제모듈에 대한 연결이 들어갈 것이다.
	- views.py
		- create
		- list
		- detail
- User가 결제한 책을 연결하기 위해서 Purchace 앱이 필요.
- Purchase Model
	- user -> 외래키로 연결
	- product -> 외래키로 연결
	- timestamp

- [ ] nav bar 만들어서 회원가입, 로그인, 등등
- [ ] 상품을 누가 만들었는지, Retrieve, Create에 대한 커스텀 모델 