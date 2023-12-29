---
aliases: 
tags: 
description:
title: Team-dev.recre-frontend.README.md {swjungle}
created: 2023-12-12T20:07:13
updated: 2023-12-12T20:53:09
---
- [[Team-dev.recre-frontend.README.md {swjungle}]]
- [recre-frontend](https://github.com/Team-def/recre-frontend)
___

## META README

본 파일은 깃허브 리포지토리 [recre-frontend](https://github.com/Team-def/recre-frontend) README.md 파일에 작성할 내용의 초안을 담았습니다. 서비스 소개, 게임 소개, 배포 방법, 포스터, 최종발표영상 까지는 recre-backend와 동일한 내용을 담을 예정이고, 그 뒤에는 발표나 포스터에서 못다 이야기한 구체적인 프론트엔드쪽 기술적인 챌린지들을 정리할 예정입니다.

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

## Build With NextJS

먼저 필요한 의존성들을 설치해줍니다.

```
npm i
```

환경변수에 필요한 .env.production과 .env.development를 생성합니다. 아래는 필요한 환경변수들을 정의한 예제입니다.

```
NEXT_PUBLIC_RECRE_URL=프론트엔드 서버 URL
NEXT_PUBLIC_BACK_API=백엔드 서버 URL
NEXT_PUBLIC_SOCKET_API=웹소켓 게이트웨이 URL
```

그리고 다음 명령어를 통해 각각 개발용과 프로덕션용 모드로 실행할 수 있습니다.

```
npm run dev -- -p <port>
npm run start -- -p <port>
```

## 포스터

![최승현-RecRe-2](https://github.com/Team-def/recre-backend/assets/18757823/533ea910-79e2-47e5-9e44-edd4bfac73f8)

## Project Directory Structure

%% 아래에 NextJS 디렉토리 구조에 관한 내용을 적어주세요 %%

**app/**

```
app
├── catch
│   └── page.tsx
├── catchAnswer
│   └── page.tsx
├── favicon.ico
├── gamePage
│   └── page.tsx
├── gameSelect
│   └── page.tsx
├── globals.css
├── layout.tsx
├── login
│   └── login.tsx
├── modules
│   ├── answerAtom.tsx
│   ├── backApi.tsx
│   ├── catchStartAtom.tsx
│   ├── gameAtoms.tsx
│   ├── loginAtoms.tsx
│   ├── loginTryNumAtom.tsx
│   ├── popoverAtom.tsx
│   ├── redGreenAtoms.tsx
│   ├── redGreenStartAtom.tsx
│   ├── socketApi.tsx
│   ├── tokenAtoms.tsx
│   └── userInfoAtom.tsx
├── page.tsx
├── player
│   └── page.tsx
├── playerComponent
│   ├── catchPlayer.tsx
│   └── redGreenPlayer.tsx
├── profile
│   └── profile.tsx
├── provider.tsx
├── redGreen
│   └── page.tsx
└── token
    └── page.tsx
```

**component/**

```
component
├── MyModal.tsx
├── MyPopover.tsx
├── OauthButtons.tsx
├── Particle.tsx
├── QRpage.tsx
├── footer.tsx
├── header.tsx
├── oauthButtonsStyle.module.css
├── rankingBoard.tsx
├── snackBar.tsx
└── youngHee.tsx
```

## 기술적 챌린지

### 호스트와 플레이어 간 캔버스 동기화

> 호스트가 호스트 화면에서 캔버스에 그림을 그리면 플레이어의 화면에도 동시에 그림이 그려지게 하고 싶었다.

#### 첫번째 방법

처음에는 캔버스의 기능 중 하나인 그림을 이미지로 바꿔서 보내는 방법을 사용해보려 했었다.

- onmouseup 일 때 호스트의 그림을 이미지로 저장하여 canvasData에 담아 ‘draw’로 emit 하는 코드.

```jsx
useEffect(() => {
    console.log(234234)
    const canvas: HTMLCanvasElement | null = canvasRef.current;
    if (canvas) {
        const context = canvas.getContext('2d');
        if (context) {
          context.beginPath();
          // 서버에 그림 데이터 및 캔버스 정보 전송
          const canvasData = {
            data: context.getImageData(0, 0, canvas.width, canvas.height).data,
            width: canvas.width,
            height: canvas.height,
          };
          socket.emit('draw', canvasData);
        }
    }
  }, [isPainting]);
```

그러나 위와 같은 방식은 호스트가 마우스로 그림을 그리다가 마우스를 뗐을 때만 그림으로 저장되어 플레이어에게 보내지며, 플레이어는 실시간 그림이 아닌 마우스를 뗐을 때의 그림만 갱신되게 되어 실시간성이 떨어지게 된다.

#### 두번째 방법

useCallback을 사용하여 호스트가 onmousemove 일 때 실시간으로 그림의 시작 지점과 그려지는 좌표를 emit하게 하여 플레이어한테도 실시간으로 호스트의 그림이 그려지게 방법을 바꾸었다.

```jsx
const paint = useCallback(
    (event: MouseEvent) => {
      event.preventDefault();   // drag 방지
      event.stopPropagation();  // drag 방지

      const newMousePosition = getCoordinates(event);
      if (isPainting) {
        if (mousePosition && newMousePosition) {
          drawLine(mousePosition, newMousePosition);
          setMousePosition(newMousePosition);
          socket.emit('draw', {
            room_id : userInfo.id,
            x : newMousePosition.x,
            y : newMousePosition.y,
            color : isEraser?'white':curColor,
            lineWidth : isEraser ? eraserWidth : lineWidth,
            first_x : mousePosition.x,
            first_y : mousePosition.y,
          });
        }
      } else {

      }
    },
    [isPainting, mousePosition]
  );
```

이렇게 수정하니 이전보다는 소켓의 emit 횟수는 늘었지만 이미지를 보내는게 아닌 좌표 정보면 보내는 것이므로 소켓에 큰 무리 없이 정보를 실시간으로 보낼 수 있었다.

또한 이런 방식으로 현재 100명까지 문제 없이 길지 않은 반응속도로 호스트의 그림이 플레이어의 화면에 동기화되는 것을 확인했으며 1000명까지도 문제 없이 소켓이 감당할 수 있는 것으로 보인다.

#### 고려해볼 부분

다만 네트워크 환경에서의 문제가 있을 수 있어 보인다.

예를 들어 100명의 플레이어와 1명의 호스트가 같은 와이파이 공유기에 접속하여 게임을 진행한다고 봤을 때, 그 네트워크가 과연 100명에게 무사히 그림 정보를 보낼 수 있을 지 고려해 봐야 할 것 같다.

### 핸드폰을 흔들어서 화면 속 캐릭터가 앞으로 나아가게 만드는 방법



