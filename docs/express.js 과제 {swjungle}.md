---
aliases: 
tags: 
description:
title: express.js 과제 {swjungle}
created: 2023-11-04T15:30:02
updated: 2023-11-04T16:19:56
---
- [[express.js]]
- [[week13 {swjugle}{team creation} {expressjs}]]
___

## SRS

> CRUD 가능한 게시판 + JWT를 활용한 회원 인증/인가기능을 포함한 백엔드 서버 만들기 (REST API)

### Mandatory

- APIs
	- 전체 게시글 조회
	- 게시글 조회
	- 게시글 작성
	- 게시글 수정
	- 게시글 삭제
	- 댓글목록 조회
	- 댓글작성
	- 댓글수정
	- 댓글삭제
	- 회원가입
	- 로그인

- data
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
