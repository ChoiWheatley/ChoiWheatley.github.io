---
description:
aliases: 
tags: 
created: 2023-03-16T18:40:57
updated: 2023-07-11T15:21:07
title: wasm-pack 사용하기
---
parent:
- [[0013 Rust 🦀]]
- [[wasm with rust]]
references:
- [wasm-pack tutorials](https://rustwasm.github.io/docs/wasm-pack/introduction.html) [[0080 Scraps 📚]]
- [wasm-pack commands](https://rustwasm.github.io/docs/wasm-pack/commands/index.html) 


# wasm-pack 으로 새 패키지 생성하기

```sh
wasm-pack new hello-wasm
```

# wasm 빌드하기

```sh
wasm-pack build --scope mynpmusername
```

# 웹상의 패키지 빌드하기

```sh
npm run build
```

# npm에 퍼블리싱하기

```sh
wasm-pack publish
```

# [wee_alloc](https://rustwasm.github.io/docs/wasm-pack/tutorials/npm-browser-packages/template-deep-dive/wee_alloc.html#wee_alloc) 할당자 사용하기

컴파일을 새로 해야 하기 때문에 만약 npm 서버가 동작중이라면 종료하고 다시 컴파일 하여야 한다.

```sh
wasm-pack build -- --features wee-alloc
```

또는 `Cargo.toml` 파일에서 기본값으로 `wee_alloc`을 추가하면 된다.

```toml
[features]
default = ["console_error_panic_hook", "wee_alloc"]
```

