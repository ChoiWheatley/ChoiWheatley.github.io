---
aliases: 
tags: 
description:
title: week14-18 {swjungle}{my own weapon}{nestjs, socketio}
created: 2023-11-10T14:54:01
updated: 2023-12-15T21:00:41
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
	- ![[Drawing 2023-12-05 19.24.48.excalidraw]]

- 2023-11-27
	- [ALB socket.io 관련 질문](https://stackoverflow.com/questions/43702043/aws-application-load-balancer-and-socket-io) | Redis adapter를 사용하여 여러 채팅룸을 만들 수 있다고 한다. 이거는 프로세스나 서버를 넘나드는 브로드캐스팅을 위한 전략이고.
	- [[typeorm]]으로 IMDB 사용할거임. 근데 오늘은 월요일이네? 발표는 수요일인데?

- 2023-11-29 수
	- 🚨 15:00 발표때 시연할 무궁화게임이 당일 오전 1시까지도 통합테스트도 불가능하다는 상황이 벌어져 발표주제를 급하게 지난주에 했던걸 그대로 하기로 결정. 발표자료는 거의 대부분을 그대로 활용할 거지만 핵심기능파트와 기술적 챌린지 파트는 수정하기로 결정. EC2 환경에서 테스팅도 진행할 예정.
	- 2023-11-29T08:43:20 지금 나에게 주어진 것에 최선을 다하자. 나에겐 발표준비 전까지 1시간 반 여 시간이 주어져있고, 놓쳤던 백엔드 개발 흐름을 **코드**위주로 되찾을 것이다. 그리고 10시가 되어 팀원들이 모이면 바로 발표준비에 돌입할 것이다. 말로만 떠들지 말자고.
		- 프론트가 없는 상황에서 백엔드 웹 소켓 api를 테스트하려면 포스트맨과 가짜 페이지를 사용하여야 한다. GPT를 사용하여 socket.io 커넥션을 요청하고 이벤트를 emit하는 단순한 기능을 가진 html 파일을 만들어보자. 

- 2023-11-30 금
	- [의도치 않은 이벤트 막기](https://github.com/Team-def/recre-backend/issues/69)
		- room 객체 쉽게 얻어오는 방법 알아내기
		- 모든 이벤트에 대하여 상태 다이어그램을 기준으로 유효성 여부를 판단한다.
	- [[room, player, host ER {swjungle} {my own weapon}]]
	- [ ] 이모티콘 날리는 이벤트 in redgreen

- 2023-12-02 토
	- ✅ build & deploy manually
		- EC2가 빌드할 필요 없게, 로컬(혹은 본가컴에서)에서 빌드하고 빌드 폴더만 scp 따위를 사용하여 업로드 한 결과를 EC2는 실행만 시키게 만들고 싶다.
		- 중요한 관건은 `.env` 혹은 `.env.local`을 어떻게 프로덕션 모드로 바꾸냐는 것에 있다.
			- [env-cmd](https://www.npmjs.com/package/env-cmd)를 활용한 방법  ⟶ 실패. 똑같이 redirect url이 본가 컴퓨터로 향한다.
		- nest build 해서 `dist/main.js` 파일을 EC2에 올리기
			- Build Server: `npm run build` 또는 `nest build --builder webpack`
			- Build Server: `scp -r dist <deploy-server-domain>:/path/to/repo` -- 이때 `deploy-server-domain`이 `~/.ssh/config` 안에 등록되어있어야 합니다.
			- Deploy Server: `npm run start:prod`
		- [[next build해서 `.next` 디렉터리를 EC2에 올리기]]
			- Build Server: `npm run build`
			- Build Server: `scp -r dist <deploy-server-domain>:/path/to/repo` -- 이때 `deploy-server-domain`이 `~/.ssh/config` 안에 등록되어있어야 합니다.
			- Deploy Server: `npm run start`
	- 자동배포
		- main에 푸시한 소스를 github action을 사용하여 자동으로 aws에 배포하게 만드려면?

- 2023-12-03 일
	- [[artillery {nodejs}]] 와 [[locust]] 사용하여 NestJS 부하분산테스트 진행하기. 특히 Socket.io 부분!
	- [노마드 코더 NextJS 이론](https://nomadcoders.co/nextjs-fundamentals) 공부하고 프론트쪽에 달라붙기 (5 frontend 체제로 젼환)

- 2023-12-04 월
	- 정신적 피로와 두통을 느끼는 중이다. [[2023-12-04]]
	- 오랜만에 팀원과 산책을 조지고 왔다. 햇살쬔게 이게 얼마만인지
	- 테스팅 보다 중요한 **[[지연시간 극복 {swjungle}{recre}]]** 에 초점을 옮겨 구현.

- 2023-12-05T20:12:46
	- 무궁화 준비 -> 준비취소 -> 준비하고 흔들면 10 넘겨도 준비가 안된다.
	- [FE] 자기가 finish라면 더 이상 run을 보내지 말아야 함.
	- [BE] run을 보낸 플레이어가 finished 상태라면 finish 루틴을 실행시키지 말아야 함. -> too much

- 2023-12-09 토
	- [[네트워크 불안정으로 재연결 로직 {swjungle}{socketio}]]
	- 없는 방에 run 요청 보내면 null exception 발생함

	```
	[Nest] 18415  - 12/09/2023, 5:02:49 PM   ERROR [WsExceptionsHandler] Cannot read properties of null (reading 'room')
	
	TypeError: Cannot read properties of null (reading 'room')
	    at RedGreenGateway.run (/home/chltm/workspace/CSH/recre-backend/src/session/redgreen.gateway.ts:321:50)
	    at RedGreenGateway.<anonymous> (/home/chltm/workspace/CSH/recre-backend/node_modules/@nestjs/websockets/context/ws-proxy.js:12:32)
	    at WebSocketsController.pickResult (/home/chltm/workspace/CSH/recre-backend/node_modules/@nestjs/websockets/web-sockets-controller.js:96:24)
	```

- 2023-12-10 일
	- nvchad nerd font 해제하는 방법?
	- 게임 세팅 글자를 바꾸자마자 alert를 띄우지 말고, 게임시작 누를때 검사하는 방식으로 바꾸면 안되나?

[[Drawing 2023-12-11 23.32.41.excalidraw]]  
![[Pasted image 20231212152458.png]]


[[Drawing 2023-12-12 00.29.48.excalidraw]]  
![[Pasted image 20231212152442.png]]

- 2023-12-12 화
	- 포스터 제출완료, 게임 두개 구현 완료, 호스트 유저 관리 및 게임 룸과 같은 게임을 돌리기 위해 필요한 기반 구현완료. 
	- 앞으로 남은 건 구현 방면에서 사용자 경험 향상과 비주얼 적인 측면 향상
	- 앞으로 남은 건 구현 외적인 부분에서 발표자료 다듬기와 발표영상 제작 (최종발표 대안)
	- 팀장인 내가 해야 할 건 발표준비. README.md 파일 작성해서 본 프로젝트의 WHY, HOW, WHAT을 어필하고 포트폴리오로 활용가치를 만들어내는 것. 실제 발표를 준비하면서 레그리에이션 강사들의 말빨을 터득하고 발표 진행중 공백이 생기지 않도록 대비한다.
	- [[Team-def.recre-backend.README.md {swjungle}]]
	- [[Team-dev.recre-frontend.README.md {swjungle}]]
	- 예상질문들
		- 캐치마인드 draw 이벤트 관련한 예상질문
			- 이벤트 발생주기가 어떻게 되나요? - 마우스 움직임 이벤트가 발생하는 주기가 곧 socketio 이벤트 전송주기인데 얼마나 자주 발생하는건지 질문이 들어올 수도 있겠다
		- 무궁화꽃이피었습니다 예상질문
			- 핸드폰을 흔들어서 캐릭터를 이동시키는 방법이 궁금하다.  
			- 애니메이션 어떻게 했나?
	- 다른 팀의 조언
		- 이 앱을 전혀 모르는 사람한테 피드백을 받아봐라. 우리자식은 예뻐보일 수밖에 없으므로 제삼자의 시선으로 객관적으로 도움받는게 좋을것. 친구나 가족한테.
	- 

- 2023-12-13 수요일 리허설 전날!
	- 레크리에이션 영상 시청 [말버스](https://www.youtube.com/watch?v=E4Dyadqf374) 후 생각난 것들
		- ~~실제 레크리에이션 진행처럼 발표를 구성하는것. 지금 생각나는 것으로는 사용된 기술스택의 로고의 일부분만 보여줘놓고 관중들이 맞추게 시키는 거 정도?~~
			- 팀원들의 의견: 진지할땐 진지하고, 시연때는 신나게 진행하고.
		- 전체적인 분위기와 텐션이 높은 상태를 유지됐음 좋겠다. 장난스러운 건 아님.
	- 레크리에이션 상황을 글로 써보자
		- 팀 소개 및 서두: 설치없이 쉽게 레크리에이션을 진행할 수 있는 RecRe
		- 필요성: 
			- WHY: 나는 세상에 자격증 있는 사람만 레크리에이션 강사를 할 수 있다고 생각하지 않음.
			- HOW: 복잡한 과정이나 엄청난 말빨 없이도 쉽게 모두의 관심을 이끌어낼 서비스를 고안
			- WHAT: 오직 웹 브라우저와 참가자 스마트폰만으로 게임하고 소통할 수 있는 플랫폼인 RecRe를 만들게 되었다.
		- 핵심기능:
			- 호스트는 PC, 플레이어는 스마트폰만 있으면 OK. 호스트가 게임을 오픈하면 QR코드가 뜨게 되고 플레이어가 카메라를 켜서 QR코드 페이지로 접속, 닉네임을 입력하고 게임을 진행하게 되는 플로우!
		- 시연:
			- 캐치마인드
				- 호스트가 그림을 그리면, 플레이어가 정답을 맞추면 되는 게임. 쉽죠?
				- 50명으로 해놓고 플레이를 진행하게 되면 기다리는 동안 분명히 시간이 붕 뜬다. 그동안 풀 썰을 마련하자. 
				- 정답자에게는 소정의 상품 제공한다는 내용 공지
			- 무궁화 꽃이 피었습니다
				- 
	- 최종발표영상
		- 구성은 발표준비한 것 그대로 가져가되, 얼굴 & 화면녹화로 발표가 진행되고, 시연파트는 오늘 저녁(20:00)에 있을 예비 리허설의 현장을 녹화 + 플레이어 화면을 옆에 띄워 게임이 어떻게 진행되는지 함께 보여주기
	- 

- 2023-12-15 금요일
	- SQLite IMDB를 사용한 이유가 무엇인가?
		- 인 메모리 DB를 사용한 첫번째 이유는 데이터들이 영속적일 필요가 없기 때문이고, 두번째로는 서버 내에서 관리중이던 데이터들의 관계가 점점 복잡해졌기 때문입니다. 대표적인 예로 게임룸 하나를 지우면 연관 호스트와 게임 룸에 등록된 플레이어들을 제거해야 하는데, IMDB를 사용하면 자동으로 제거해주기 때문입니다.
