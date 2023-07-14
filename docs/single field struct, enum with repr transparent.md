---
description:
aliases: 
tags: 
created: 2023-03-29T21:27:39
updated: 2023-07-11T15:20:18
title: single field struct, enum with repr transparent
---
- https://doc.rust-lang.org/nomicon/other-reprs.html#reprtransparent
- 메모리 구조에 대한 명시를 담는 속성이 `#[repr]` 이다.
- `[repr(transparent)]` 속성은 데이터 사이즈가 0보다 큰 오직 하나만의 필드 혹은 variant를 담은 struct, enum에 대하여 ABI의 메모리 구조가 단일 필드와 동일함을 보장한다. 