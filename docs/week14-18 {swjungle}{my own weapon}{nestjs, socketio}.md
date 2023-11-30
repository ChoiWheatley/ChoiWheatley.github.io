---
aliases: 
tags: 
description:
title: week14-18 {swjungle}{my own weapon}{nestjs, socketio}
created: 2023-11-10T14:54:01
updated: 2023-11-29T09:02:50
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

- [[2023-11-16 socket.io {튜토리얼} {room} {cluster}]]

- 2023-11-17
	- [[HTTPS EC2 {devops}]]
	- [ ] ~~@박동호 EC2 SSH 연결하게 프라이빗키 부탁하기 (아니면 `ssh-keygen`)~~

 - 2023-11-18
	- [ ] [api nestjs chat websockets](https://wanago.io/2021/01/25/api-nestjs-chat-websockets/) 튜토리얼

- 2023-11-18
	- [ ] github issue 왜 다들 안쓰지.. 노션에 데이터베이스를 더 선호하는 것 같은데 깃허브 이슈 사용문화를 스며들게 만들기 위해선 뭘 하는것이 좋을까?
	- [[2023-11-18 auth-redirect {swjungle}{nestjs}]]

- 2023-11-19
	- [x] cookie 로컬 스토리지에 저장하는 코드 별개의 PR로 만들어버리자. 

- 2023-11-23
	- [Application Load Balancer를 사용하여 web socket 로드밸런서 연결하기](https://stackoverflow.com/questions/39336033/does-an-application-load-balancer-support-websockets) ⟶ 실패하여 그냥 집컴으로 중간발표 진행함.
	- [[중간발표 피드백, 회고, 앞으로, 코어타임 변동, 기술적 챌린지 고민 나 혼자 요약ver {swjungle}]]

- 2023-11-25
	- [[web socket ec2 {devops}]]
	- [[imdb with NestJS]]

- 2023-11-26
	- [[무궁화꽃이피었습니다 {swjungle}{my own weapon}]]

- 2023-11-27
	- [ALB socket.io 관련 질문](https://stackoverflow.com/questions/43702043/aws-application-load-balancer-and-socket-io) | Redis adapter를 사용하여 여러 채팅룸을 만들 수 있다고 한다. 이거는 프로세스나 서버를 넘나드는 브로드캐스팅을 위한 전략이고.
	- [[typeorm]]으로 IMDB 사용할거임. 근데 오늘은 월요일이네? 발표는 수요일인데?

- 2023-11-29 수
	- 🚨 15:00 발표때 시연할 무궁화게임이 당일 오전 1시까지도 통합테스트도 불가능하다는 상황이 벌어져 발표주제를 급하게 지난주에 했던걸 그대로 하기로 결정. 발표자료는 거의 대부분을 그대로 활용할 거지만 핵심기능파트와 기술적 챌린지 파트는 수정하기로 결정. EC2 환경에서 테스팅도 진행할 예정.
	- 2023-11-29T08:43:20 지금 나에게 주어진 것에 최선을 다하자. 나에겐 발표준비 전까지 1시간 반 여 시간이 주어져있고, 놓쳤던 백엔드 개발 흐름을 **코드**위주로 되찾을 것이다. 그리고 10시가 되어 팀원들이 모이면 바로 발표준비에 돌입할 것이다. 말로만 떠들지 말자고.
		- 프론트가 없는 상황에서 백엔드 웹 소켓 api를 테스트하려면 포스트맨과 가짜 페이지를 사용하여야 한다. GPT를 사용하여 socket.io 커넥션을 요청하고 이벤트를 emit하는 단순한 기능을 가진 html 파일을 만들어보자. 

- 2023-11-30 금