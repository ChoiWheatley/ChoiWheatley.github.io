---
aliases: 
tags: 
description:
title: middleware {expressjs}
created: 2023-11-03T08:39:24
updated: 2023-11-03T08:54:20
---
- [[express.js]]
- <https://expressjs.com/en/resources/middleware.html>
___

## 왜씀?

튜토리얼 따라할 때 `app.js` 바깥에서 별도의 라우터를 선언하고 해당 객체를 임포트해 `app.use()`했었다. 이때 내가 선언한 라우터와 그 모듈이 바로 미들웨어가 된다. 미들웨어 덕분에 라우팅, 요청 body 파싱, CORS, 에러 핸들링 등등 Request ⇄ Request 양 끝 사이에 일어나는 수많은 일처리를 별개의 모듈로 분리할 수 있게 되었다.

## 만들어보자

- [ ] 모든 요청을 로깅하는 미들웨어 (마치 장고의 그것과 같이)
