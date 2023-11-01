---
aliases: 
tags: 
description:
title: express.js
created: 2023-11-01T16:02:37
updated: 2023-11-02T00:11:42
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

타 모듈(미들웨어)를 app에 등록시키는 과정

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

wsl에 mongodb 설치를 하는데 애를 먹고 있다. [다음 askubuntu.com](https://askubuntu.com/questions/1379425/system-has-not-been-booted-with-systemd-as-init-system-pid-1-cant-operate)의 내용을 읽어보면, WSL은 기본값으로 systemd 사용을 막아두고 있다. 그래서 몇가지 방법을 제안했는데, 그 중에서 가장 간편해 보이는 방법인 docker를 활용해 보려고 한다.

<https://www.mongodb.com/docs/manual/tutorial/install-mongodb-community-with-docker/>

설치하려고 봤더니 WSL distro 안에서 `docker` 앱을 사용할 수 없다고 나온다. 분명히 예전에 WSL 안에서 도커 쓴 기억이 있어서 좀 찾아봤더니...

<https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers>

도커 데스크톱 설정에서 `Settings > Resources > Wsl Integration` 탭으로 가서 원하는 디스트로를 토글하면 도커 명령어를 사용할 수 있게 만들어준다고!
