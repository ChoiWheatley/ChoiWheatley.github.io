---
aliases: 
tags: 
description:
title: week14-18 {swjungle}{my own weapon}{nestjs, socketio}
created: 2023-11-10T14:54:01
updated: 2023-11-16T15:57:53
---
- [[0120 swjungle 🤖]]
- [swjungle-week14-nestjs](https://github.com/ChoiWheatley/swjungle-week14-nestjs) 공부하는 리포지토리
- [Team-def/recre-backend](https://github.com/Team-def/recre-backend) 프로젝트 리포지토리
___

## README

본 페이지는 개인적으로 공부하고 아카이빙 용도로만 활용될 듯 합니다. 회의록은 노션에 정리하고 프로젝트 진행하면서 쌓을 갖은 문서들은 깃허브에 올릴 예정입니다.

## Nest.js

[[0018.1 Nest.js 🪺]]

## Daily Notes

- [x] [[git commit message 규칙]] 알려줘야겠다.
- [[2023-11-15 {swjungle}{recre}]]
	- [[socket.io]] 게임방 구상 + 해당 프레임워크 찍먹 
	- `socket.emit` ↔️ `socket.on`
	- `io.emit`과 `socket.broadcast.emit`과의 차이
- [[2023-11-16 {swjungle}{recre}]]

### 2023-11-16 socket.io {튜토리얼} {room} {cluster}

- [[socket.io]]
	- [튜토리얼 페이지](https://socket.io/get-started/chat) 에 있는 과제(?) 직접 구현 
	- [NestJS websockets adapters](https://docs.nestjs.com/websockets/adapter#extend-socketio) 페이지에 있는 socket.io 어댑터 확인하기
	- room 방식으로 여러 호스트의 세션을 구분할건지, cluster 방식으로 포트번호와 별개의 프로세스 방식으로 세션을 구분할건지 각각의 장단점 알아내기
- 오늘 @18:00에 오승철 멘토님과 면담있음. 대화주제에 대해서 생각해보자.
	- 우리가 만들고자 하는 것에 대한 브리핑: 레크리에이션을 웹 환경으로 옮겨놓아보려고 한다. 호스트가 세션을 열면 QR을 통해 참가자들이 들어오는 구조. 

[kahoot clone nodejs {GH}](https://github.com/ethanbrimhall/kahoot-clone-nodejs/blob/master/server/server.js) 코드를 참조하여 호스트 참가, 게임생성, 참여자 참가, 게임 시작까지의 루틴을 확인했다.

**game start**

![[Pasted image 20231116151003.png]]

추가: 리디렉션을 안해도 된다. QR코드, 대기화면 아래에 게임루틴을 함께 로드할 수 있기 때문.

NestJS 코드로 넘어가자.
