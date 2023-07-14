---
description:
aliases: 
tags: 
created: 2023-04-18T17:52:04
updated: 2023-07-11T15:21:09
title: axum routing Router
---
- [`axum::routing::Router` ](https://docs.rs/axum/latest/axum/struct.Router.html)
- router type can compose handler(methods) and services.
```rust
let routes = Router::new().route(
	"/hello", // path
	axum::routhing::get(|| async { // method router
		Html("hello world")
	}),
);
```
- [[axum routing method_routing get|get]] can handle router handler as `GET` method
- 
