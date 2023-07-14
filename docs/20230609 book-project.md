---
description:
aliases: 
tags: 
created: 2023-06-09T21:28:26
updated: 2023-07-11T15:21:10
title: 20230609 book-project
---
- [[django with ajax]]부터 이어서 진행한다. ajax로 뭘 할 지 몰라서 아예 [이슈](https://github.com/ESTsoft-Book-Project/bookstore/issues/17)를 만들어 관리하기로 했다.
- [[PATCH {HTTP}]]
	- `<domain>/users/profile` 엔드포인트를 관리하는 `profile` GET 함수는 일단 `HttpResponse`를 리턴한다. 하지만 그 안에 ajax 코드가 있는데, 걔가 "replace", "remove"를 서버에 요청한다.