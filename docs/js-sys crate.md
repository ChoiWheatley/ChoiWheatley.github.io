---
description:
aliases: 
tags: 
created: 2023-03-23T22:53:45
updated: 2023-07-11T15:21:08
title: js-sys crate
---
- https://crates.io/crates/js-sys
- https://docs.rs/js-sys/0.3.61/js_sys/

Bindings for all JS global objects and functions in all JS environments like Node.js and browsers, built on `#[wasm_bindgen]` using the `wasm-bindgen` crate. This does _not_ include any Web, Node, or any other JS environment APIs. Only the things that are guaranteed to exist in the global scope by the ECMAScript standard.

JS 전역 객체들을 러스트에서 사용할 수 있게 일대일 바인딩을 해놓았다고 한다. 유의할 점으로, JS는 `camelCase` 컨벤션을 사용하지만, Rust는 `snake_case` 컨벤션을 사용하고, 이로 인해 JS 객체이름이 러스트 식으로 변경되었다.