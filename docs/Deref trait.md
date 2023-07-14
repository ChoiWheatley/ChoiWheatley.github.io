---
description:
aliases: 
tags: 
created: 2023-04-01T01:18:01
updated: 2023-07-11T15:21:09
title: Deref trait
---
- https://doc.rust-lang.org/book/ch15-02-deref.html
- https://doc.rust-lang.org/std/ops/trait.Deref.html#more-on-deref-coercion
- 그냥 단순 `*` 연산을 수행해 주는 녀석인 줄로만 알았는데 [[option as_deref]] 에서 실험을 해 보니까 연쇄적으로 deref를 해주는 것 같다.