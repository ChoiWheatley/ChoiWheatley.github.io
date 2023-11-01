---
aliases: 
tags: 
description:
title: express.js
created: 2023-11-01T16:02:37
updated: 2023-11-01T16:39:37
---
- [[0018 Javascript ☕️]]
___

## README

node.js 환경의 웹 개발 프레임워크중 하나인 express를 공부하면서 배운 내용을 아카이브 합니다. 빠른 지식습득을 위해 과제를 먼저 진행하고 부족한 부분은 그때그때 찾아가는 식으로 진행할 예정입니다.

## Structure of express.js

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
