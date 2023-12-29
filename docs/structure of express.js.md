---
aliases: 
tags: 
description:
title: structure of express.js
created: 2023-11-03T19:24:40
updated: 2023-11-03T19:24:44
---

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
