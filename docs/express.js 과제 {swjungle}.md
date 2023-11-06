---
aliases: 
tags: 
description:
title: express.js 과제 {swjungle}
created: 2023-11-04T15:30:02
updated: 2023-11-06T19:10:24
---
- [[express.js]]
- [[week13 {swjugle}{team creation} {expressjs}]]
- <https://github.com/ChoiWheatley/swjungle-week13>
___

## SRS

> CRUD 가능한 게시판 + JWT를 활용한 회원 인증/인가기능을 포함한 백엔드 서버 만들기 (REST API)

### Mandatory

- APIs
	- [x] 전체 게시글 조회
	- [x] 게시글 조회
	- [x] 게시글 작성
	- [x] 게시글 수정
	- [x] 게시글 삭제
	- [x]  댓글목록 조회
	- [x]  댓글작성
	- [x]  댓글수정
	- [x]  댓글삭제
	- [x] 회원가입
	- [x] 로그인

- Post
	- title
	- content
	- author
	- createdAt
	- updatedAt
- Comment
	- content
	- author
	- createdAt
	- updateAt
- User
	- nickname
	- password ~~비밀번호를 그대로?~~

## milestone

> 게시글 + 댓글 회원없이 연결 ⟶ 회원관리 with [[Securities about {https} and {jwt {cookie}, {session}}|JWT]] ⟶ Post, Comment에 `author` FK로 변경 ⟶ 배포

### 게시글 + 댓글 회원없이 연결

- ~~회원없이 CRUD 가능한 Post, Comment 빠르게 구현하고 DB에 잘 들어가는지 테스트.~~

- Post **has many** Comment 관계
	- 일단 migration이 필요함. 스키마를 바꿔야한다.
	- Comment
		- ...
		- postId (FK)

	 ```mermaid
		erDiagram
		Post ||--o{ Comment : ""
	```

	- 바뀌는 Posts api routing
		- `GET /post/:postId/comment`  
			- response: 

			```json
			"data": [
				{
					"author": "ChoiWheatley",
					"commentId": 2,
					"content": "my first comment",
					"createdAt": "2023-11-05T14:29:44.000Z",
					"updatedAt": "2023-11-05T14:29:44.000Z"
				},
				{
					"author": "ChoiWheatley",
					"commentId": 2,
					"content": "my first comment2",
					"createdAt": "2023-11-05T14:29:44.000Z",
					"updatedAt": "2023-11-05T14:29:44.000Z"
				}
			]
			```

			- 저장은 잘 됐는데, 이걸 어떻게 꺼낼 수 있을지 모르겠다. [fetching associations](https://sequelize.org/docs/v6/core-concepts/assocs/#fetching-associations---eager-loading-vs-lazy-loading) 참고

### 회원가입 / 로그인 / 로그아웃
