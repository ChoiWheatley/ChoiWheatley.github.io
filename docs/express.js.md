---
aliases: 
tags: 
description:
title: express.js
created: 2023-11-01T16:02:37
updated: 2023-11-03T09:04:29
---
- [[0018 Javascript ☕️]]
- [Node.js 교과서 개정 3판 - 웹 아카이브 링크](https://thebook.io/080334/)
- <https://expressjs.com/>
___

## README

node.js 환경의 웹 개발 프레임워크중 하나인 express를 공부하면서 배운 내용을 아카이브 합니다. 빠른 지식습득을 위해 과제를 먼저 진행하고 부족한 부분은 그때그때 찾아가는 식으로 진행할 예정입니다.

**목표**

- HTTP 요청에 대하여 적절한 응답을 보낼 수 있다.
- 데이터베이스(처음엔 MongoDB, 나중엔 MySQL)에 ORM을 사용하여 쿼리문을 날릴 수 있다.
- 요구사항에 따라 게시판 사이트를 위한 백엔드 서버를 만들 수 있다.

## Structure of express.js

### file structure

```
.
├── app.js
├── package-lock.json
├── package.json
├── node_modules
│   └── ...
└── routes
    └── goods.js
```

- **`app.js`** main 루틴이 들어있는 곳. 다른 모듈을 임포트(`require`)하여 URL을 라우팅 할 수 있다.
- **`package-lock.json`** 설치된 패키지 메타데이터들을 저장한 파일, 이 파일만 있으면 다른 개발자도 같은 패키지 설정을 공유할 수 있다.
- **`package.json`** 패키지 메타데이터를 정의한 파일. 
- **`node_modules`** npm을 통해 패키지 의존성들이 들어있는 디렉토리
- **`routes`** 내가 임의로 생성한 디렉토리. 하위 모듈들을 담는 디렉토리.

### app.js

express 객체와 다른 모듈들을 임포트

- [?] 그런데, `require`도 임포트인데, `import`를 쓰지 않는 이유가 뭘까?

```js
const express = require("express");
const goodsRouter = require("./routes/goods");
```

express app 실행 및 listening

```js
const app = express();

app.listen(port, () => {
  console.log(port, "포트로 서버가 열렸어요! ♥️");
});
```

라우팅

<https://expressjs.com/en/starter/basic-routing.html> 참조

```js
app.get("/", (_req, res) => {
  res.send("Hello, World!");
});
```

타 모듈([[middleware {expressjs}]])를 app에 등록시키는 과정

<https://expressjs.com/en/starter/static-files.html> 참조

```js
app.use(express.json());
app.use("api/", [goodsRouter]);
```

### routes/goods.js module

라우터 미들웨어를 사용하기 위해 객체를 받아오는 코드

```js
const router = express.Router();
```

라우터 export를 해야 타 모듈에서 볼 수 있다.

```js
module.exports = router;
```

## mongodb

- [mongoose docs](https://mongoosejs.com/docs/)
- [mongodb docs](https://www.mongodb.com/docs/)

wsl에 mongodb 설치를 하는데 애를 먹고 있다. [다음 askubuntu.com](https://askubuntu.com/questions/1379425/system-has-not-been-booted-with-systemd-as-init-system-pid-1-cant-operate)의 내용을 읽어보면, WSL은 기본값으로 systemd 사용을 막아두고 있다. 그래서 몇가지 방법을 제안했는데, 그 중에서 가장 간편해 보이는 방법인 docker를 활용해 보려고 한다.

<https://www.mongodb.com/docs/manual/tutorial/install-mongodb-community-with-docker/>

도커를 WSL2에서 사용할 수 없길래 좀 찾아봤다. [[Use docker in WSL2 distro]]

도커로 mongodb 실행시킨 뒤에 연결까지 확인했으나 127.0.0.1:27017로 연결이 안되길래 포트가 열려있지 않았음을 알았다. 그래서 포트 옵션 `-p`를 줬더니 돌아가는 것을 확인했다.

```
docker run --name mongodb -d -p 27017:27017 mongodb/mongodb-community-server:latest
```

![[스크린샷 2023-11-02 011628.png]]

그리고 이번에도 마찬가지로 [[port forwarding WSL2]]에서 명시한 바와 같이 파워쉘 스크립트에 포트 27017을 추가하여 원격지에서도 웹 브라우저로는 물론 3T로도 연결해 볼 수 있게 되었다.

**튜토리얼 흐름**

goods를 추가할 수 있는 HTTP POST 요청을 만들어보자. app.js 안에서 connect 함수를 불러온다. 그리고 `schemas/goods.js` 안에 도큐먼트 스키마를 정의하고, 정의한 스키마대로 데이터를 JSON 형식으로 읽어들이고 mongodb에 도큐먼트를 추가하는 코드를 `routes/goods.js` 안에 넣는다. 그러면 이제 사용자가 다음과 같이 요청을 날리게 된다면...

- request: 
	- ![[스크린샷 2023-11-02 오후 4.32.29 1.png]]
- response:
	- ![[Pasted image 20231102164255.png]]

지금 현재 mongodb는 별도의 인증절차가 없다. 지금 Compass를 통해서 데이터베이스들을 확인하고 있는데, 내가 만들지 않은 "READ__ME_TO_RECOVER_YOUR_DATA"라는 데이터베이스가 생성됐음을 확인했다. 당신의 데이터가 암호화 되었으니 0.01 BTC를 달라는 내용의 도큐먼트와 함께. 근데 정작 다른 데이터베이스들은 접근이 가능했다. 

- [ ] mongodb 인증절차 추가하기

- [?] 언제는 `return res.json(...)`를 하고 언제는 그냥 `res.json(...)`만 하고 뭐냐?
	- 전자: POST 요청이 유효하지 않은 경우
	- 후자: GET, POST 정상적인 리턴일때

**장바구니에 상품 추가**

**장바구니 상품 수량 변경**

PUT 요청을 받아 `$set`으로 업데이트.  <https://www.mongodb.com/docs/manual/reference/operator/update-field/>

```js
await Cart.updateOne({ goodsId: goodsId }, { $set: { quantity } });
```

**장바구니 조회**
