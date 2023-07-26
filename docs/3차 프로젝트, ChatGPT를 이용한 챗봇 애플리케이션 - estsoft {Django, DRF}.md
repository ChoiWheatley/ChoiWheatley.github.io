---
aliases: 
tags: 
description:
title: 3차 프로젝트, ChatGPT를 이용한 챗봇 애플리케이션 - estsoft {Django, DRF}
created: 2023-07-26T09:38:10
updated: 2023-07-26T15:35:07
---
- [요구사항 {Notion}](https://paullabworkspace.notion.site/ChatGPT-1bc750970cef40519e42a9d74404b5cb)
- [ormi-project-1 {Notion}](https://github.com/ChoiWheatley/ormi-project-1)

## 기본 요구사항

- 모든 구현은 **DRF** 를 이용하여 구현.
- 클래스 기반 뷰 사용
- 회원가입 / 로그인
- 채팅은 로그인을 한 유저만 사용 가능함
- 유저당 하루 최대 5번 요청가능
- 채팅내역을 DB에 저장
- 저장된 채팅 내역을 조회할 수 있고,
- 모든 채팅내역은 로그인하지 않은 유저들에게도 열람권한이 있다.
- ERD를 그려주세요.

## 선택 요구사항

- FE 배포 (Github Page를 사용하여 별개의 프로젝트로)
- BE 배포
- HTTPS 추가
- kakao, github 등 OAuth2 추가하기

## Diagrams

기본 요구사항: FE와 BE 분리: 프론트엔드에서 백엔드 서버를 통해 요청을 보내준다.

### Flowchart

```mermaid
flowchart LR
	id1[Client]
	id2[FE]
	id3[BE]
	id4[OpenAI]
	id5[DataBase]

	id1 -- 1 --> id2
	id2 -- 2 --> id1
	id1 -- 3 --> id3
	id3 <-- 4 --> id4
	id3 <-- 4 --> id5
	id3 -- 5 --> id1
```

1. 클라이언트가 프론트에 HTML 문서를 요청함.
2. 프론트는 JS코드가 담긴 HTML 문서를 반환하고 클라이언트는 문서를 렌더링함.
3. 클라이언트는 백엔드에게 다음과 같은 요청들을 보낸다.
	1. 회원가입, 로그인, 로그아웃과 같은 사용자 인증/인가
	2. 모든 유저의 채팅내역
	3. 본인의 채팅내역
	4. 챗봇과의 채팅세션
4. 백엔드는 클라이언트의 요청에 따라서 두 가지 서비스에 요청을 보낸다. 유스케이스 그래프는 따로
	1. DB
	2. OpenAI
5. 백엔드가 각각의 서비스로부터 응답을 받으면 이를 통해 클라이언트에게 응답을 전송함.

### Usecase Diagram

![[chatgpt-usecase.excalidraw]]

- _User_
	- Request Chat List
		- _System_
			- Retrieve chat data.
			- Create DOM element for display
	- Request Chat Session
		- _System_
			- Check if the user is logged in
			- Create Request Prompts
				- _ChatBot_
					- Ask user's current state
					- Ask user's goal
					- Ask user's other requirements
