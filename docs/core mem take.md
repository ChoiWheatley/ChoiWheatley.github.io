---
description:
aliases: 
tags: 
created: 2023-03-31T19:57:29
updated: 2023-07-11T15:21:09
title: core mem take
---
- [`take`](https://doc.rust-lang.org/nightly/core/mem/fn.take.html)
- Replaces `dest` with the default value of `T`, returning the previous `dest` value
- 말 그대로 변수가 가지고 있던 값을 훔치고 원래 있던 자리엔 기본값으로 채워넣겠다는거네.
- [[option take]] 메서드를 사용하면 `Option` 타입 변수가 가지고 있던 값을 훔치는 것이 가능하다!