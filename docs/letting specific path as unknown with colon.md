---
description:
aliases: 
tags: 
created: 2023-04-18T22:36:59
updated: 2023-07-15T21:33:04
title: letting specific path as unknown with colon
---
- [Route paths](https://expressjs.com/en/guide/routing.html#route-paths)
- [Route parameters](https://expressjs.com/en/guide/routing.html#route-parameters)
	- 라우팅 path에 placeholder를 서버에서 지정하면 호출하는 쪽에서 구체적인 값으로 치환하여 URL을 완성하여 보낼 수 있다.
	- Route path: `/users/:userId/books/:bookId/`
	- Request URL: `http://localhost:8080/users/34/books/523448`
	- `req.params: { "userId": "34", "bookId" : "523448" }`
- [[걍 라우팅 전반적인 내용에 대한 기본부터 숙지해라]]
