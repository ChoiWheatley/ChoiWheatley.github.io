---
description:
aliases: 
tags: 
created: 2023-04-07T01:40:25
updated: 2023-07-11T15:21:09
title: array_zip is unstable
---
- https://github.com/rust-lang/rust/issues/80094
- 두 `i32` 배열을 받아다 element-wise하게 더하여 새 배열을 만들어 보고 싶었으나 `zip`이 아직 불안정하다.
- 그래서 배열을 이터레이터로 만든 다음에 [[collect into array]] 해 보려고 했으나 이마저도 안된다고 한다.