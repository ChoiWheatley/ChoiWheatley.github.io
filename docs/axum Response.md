---
description:
aliases: 
tags: 
created: 2023-04-20T22:24:45
updated: 2023-07-15T21:30:21
title: axum Response
---
- https://docs.rs/axum/latest/axum/response/index.html#
- axum은 모든 핸들러(라우터의 인자로 넘겨주었던)가 전부 `Response` 타입 객체를 반환하게끔 구조를 정의했다. 
- 따라서 `IntoResponse` 트레잇을 구현한 객체라면 전부 핸들러가 반환할 수 있게 되는 것이다.
- 
