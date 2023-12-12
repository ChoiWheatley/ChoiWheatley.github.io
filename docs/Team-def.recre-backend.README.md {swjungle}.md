---
aliases: 
tags: 
description:
title: Team-def.recre-backend.README.md {swjungle}
created: 2023-12-12T15:26:15
updated: 2023-12-12T16:27:07
---
- [[week14-18 {swjungle}{my own weapon}{nestjs, socketio}]]
___

## META README

본 파일은 깃허브 리포지토리 [recre-backend](https://github.com/Team-def/recre-backend) README.md 파일에 작성할 내용의 초안을 담았습니다. 서비스 소개, 게임 소개, 배포 방법, 포스터, 최종발표영상 까지는 recre-frontend와 동일한 내용을 담을 예정이고, 그 뒤에는 발표나 포스터에서 못다 이야기한 기술적인 챌린지들을 정리할 예정입니다.

## RecRe

- \[WHY\] 우리 모두는 관중들의 이목을 집중시키고 분위기를 환기할 수 있는 힘을 가지고 있습니다.
- \[HOW\] Team-def는 별도의 지식 없이도 웹 브라우저만으로 쉽게 레크리에이션을 진행할 수 있는 서비스를 고안하였으며,
- \[WHAT\] 최대 100명의 관중들과 온/오프라인에서 실시간으로 소통할 수 있는 서비스 RecRe를 만들었습니다.

## Screenshots

- Catch My Mind

![나만무_중간발표_최승현](https://github.com/Team-def/recre-backend/assets/18757823/087356a6-d506-4a86-94b3-c4fd178cbf31)

- Red Light, Green Light

![나만무_중간발표_최승현](https://github.com/Team-def/recre-backend/assets/18757823/884b6614-6648-4cd7-920e-9b80771537ce)

## Architectures

![[최종 수정.png]]

- Backend
  - 호스트 유저 정보 관리: PostgreSQL
  - 실시간 연결: Socket.io
  - 플레이어, 호스트, 게임룸 상태 관리: SQLite (In Memory)
  - 백엔드 업무로직: NestJS
- Frontend
  - 프레임워크: NextJS w/ ReactJS
  - 상태관리: Jotai
  - 캐치마인드 게임; Socket.io
  - 무궁화꽃이 피었습니다 게임: Three.JS, Socket.io

## Build with NestJS

먼저 필요한 의존성들을 설치합니다.

```
npm i
```

다음으로 환경변수들을 `.env` 파일에 정의합니다. 다음 변수들이 필요합니다:

```
# 호스트 유저정보를 저장할 DB서비스

DB_HOST=
DB_USER_PASSWORD
DB_USER_NAME
DB_DATABASE
DB_PORT

# 구글 소셜로그인을 위해 필요한 클라이언트 ID

GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
GOOGLE_CALLBACK_URL=

KAKAO_CLIENT_ID=
KAKAO_CLIENT_SECRET=
KAKAO_CALLBACK_URL=

NAVER_CLIENT_ID=
NAVER_CLIENT_SECRET=
NAVER_CALLBACK_URL=

# 로그인된 호스트들을 인가하기 위한 JWT 토큰과 관련한 정보

JWT_ACCESS_TOKEN_SECRET=
JWT_ACCESS_TOKEN_EXPIRATION_TIME=
JWT_REFRESH_TOKEN_SECRET=
JWT_REFRESH_TOKEN_EXPIRATION_TIME=

# 클라이언트 서버의 주소

CLIENT_URL=

# 프론트/백엔드 공통적으로 사용될 도메인 이름, 예를 들어 www.recre.com이 있다면, recre.com이 DOMAIN입니다

DOMAIN=

# 본 서비스가 동작할때 Listen할 포트번호

LISTEN_PORT=
```

그리고 다음 명령어를 통해 각각 개발용과 프로덕션용 모드로 실행할 수 있습니다. NestJS 커맨드에 대한 자세한 설명은 [공식문서](https://docs.nestjs.com/first-steps)를 참고하세요.

```
npm run start:dev
npm run start:prod
```

## Project Directory Structure

NestJS는 Controller & Service 구조로 이루어져 있으며, 각각의 컴포넌트들이 Module 단위로 분리되어 있습니다. 아래 Tree는 실제 파일들의 구조를 간략하게 소개한 텍스트입니다.

```
src
├── app.controller.ts
├── app.module.ts
├── app.service.ts
├── auth ############################## 호스트 사용자 인증 / 인가 모듈
│   ├── auth.controller.ts
│   ├── auth.module.ts
│   └── auth.service.ts
├── main.ts
├── session ########################### 캐치마인드, 무궁화꽃이 피었습니다 게임로직 & Socket.io 인터페이스
│   ├── catch.gateway.ts
│   ├── redgreen.gateway.ts
│   ├── session.guard.ts
│   ├── session.module.ts
│   └── socket.extension.ts
├── session-info ###################### 게임, 플레이어, 호스트 상태를 관리하는 모듈
│   ├── entities
│   │   ├── catch.game.entity.ts
│   │   ├── catch.player.entitiy.ts
│   │   ├── host.entity.ts
│   │   ├── player.entity.ts
│   │   ├── redgreen.game.entity.ts
│   │   ├── redgreen.player.entity.ts
│   │   └── room.entity.ts
│   ├── session-info.module.ts
│   └── session-info.service.ts
└── user ############################## 호스트 정보를 관리하는 모듈
    ├── dto
    │   ├── create-user.dto.ts
    │   └── update-user.dto.ts
    ├── entities
    │   └── user.entity.ts
    ├── user.controller.ts
    ├── user.module.ts
    └── user.service.ts
```

## 기술적 챌린지

### 소켓 유령 제거 & 지속적인 연결성 보장

- 현상: 식별되지 않은 연결들이 게임이 종료된 이후에도 남아 서버 메모리를 낭비하는 일이 발생
- 원인1([소켓 지박령 #20](https://github.com/Team-def/recre-backend/issues/20)): 플레이어들이 QR을 통해 페이지에 접속하자마자 웹 소켓이 연결됨. 플레이어가 모종의 이유로 레디하지 못한 경우, 게임이 종료되어도 플레이어 클라이언트가 지속적으로 웹 소켓 연결을 시도하려고 하기 때문에 연결 해제가 불가능. 
	- 해결: 플레이어가 닉네임을 입력하고 준비완료 버튼을 눌러야 웹 소켓 연결을 체결하도록 타이밍을 미룸.
- 원인2([게임 결과 처리 #8](https://github.com/Team-def/recre-backend/issues/8)): 플레이어가 새로고침을 하거나 탭을 닫는 행위가 명시적 disconnection이 이루어지지 않았음.
	- 해결: 플레이어 클라이언트의 자원이 해제되는 상황에서 `leave_game` 이벤트를 명시적으로 날려 서버가 해당 소켓을 disconnect하고 또한 호스트 클라이언트에게도 `player_list_remove`를 보내어 예외처리를 수행함.

### 게임 공정성을 위한 지연시간 극복
