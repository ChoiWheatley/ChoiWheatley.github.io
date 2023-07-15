---
description:
aliases: 
tags: 
created: 2023-04-18T17:59:47
updated: 2023-07-15T21:30:21
title: axum service
---
- [into_make_service](https://docs.rs/axum/latest/axum/routing/struct.Router.html#method.into_make_service)
- router 혼자만으로 서비스를 `serve`할 수 없다. 반드시 `Server` 객체에게 넘겨주어야 `serve`가 가능하다. [[hyper Server]]
- `Server` 객체는 그렇다면 전역객체인가? 싱글톤처럼 사용하면 되는걸까?
