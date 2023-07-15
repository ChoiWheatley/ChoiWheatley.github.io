---
description:
aliases: 
tags: 
created: 2023-04-05T17:19:12
updated: 2023-07-15T21:33:03
title: Result that accepts any Errors - rust
---
- http://stevedonovan.github.io/rust-gentle-intro/6-error-handling.html

다음 타입은 모든 종류의 에러에 대응한다.

```rust
Result<(), Box<Error>>
```
