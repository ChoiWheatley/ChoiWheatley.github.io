---
aliases: 
tags: 
description:
title: 2024-03-05 wishfunding NestJS 기본 API 구현
created: 2024-03-05T16:45:45
updated: 2024-03-105T3126:09:57:48
---
- [[0018.1 Nest.js 🐱]]
---
파일 구조는 다음과 같다:

```
src
├── app.controller.spec.ts
├── app.controller.ts
├── app.module.ts
├── app.service.ts
├── entities
│   └── user.entity.ts
├── enums
│   └── friendStatus.enum.ts
├── features
│   ├── funding
│   │   ├── funding.controller.spec.ts
│   │   ├── funding.controller.ts
│   │   ├── funding.module.ts
│   │   ├── funding.service.spec.ts
│   │   └── funding.service.ts
│   └── user
│       └── user.module.ts
└── main.ts

6 directories, 13 files
```

모듈 디렉토리 안에 다 들어가 있는 방식이 아니라 features 디렉토리 안에는 업무 로직들 (Controller, Service)이 들어가 있고, 그 외에 세부사항들을 밖으로 빼놓은 것을 볼 수 있다. 대표적으로 entities, enums가 있다.

나는 funding 기능에 대한 API를 마련하기로 결정했다. 사람들이 만드는 방식이 조금씩 다를 수 있기 때문에 코드를 올리고 리뷰할 때 차이점을 발견할 수 있을 것이다.

- [ ] DB Glossary 내가 굿노트에 정리해 놓았던 거 추가하기

`/api` 엔드포인트를 NestJS에 일일이 추가할 필요는 없을 것 같다. LoadBalancer가 알아서 처리하도록 만드는 것이 좋지 않을까?

- [[Pagination in {Nest.js}]]
- [[Infinite Scrolling Pagination with JavaScript and a REST API]]
