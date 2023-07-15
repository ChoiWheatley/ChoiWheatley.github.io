---
description:
aliases: 
tags: 
created: 2023-04-20T23:57:37
updated: 2023-07-15T21:33:04
title: layer and middleware in axum
---
- https://docs.rs/axum/0.6.16/axum/middleware/index.html
- https://youtu.be/XZtlD_m59sM?t=1247
- 튜토리얼에서 모든 Request들을 커스텀 `request_mapper`라는 레이어에 통과시키는 것을 보고 기가 찼다. `map` 이라는 말이 아주 직관적으로 와닿는게, Request을 consume하여 다른 Request를 produce하기 때문이다. 러스트 문서에 이르면, 미들웨어를 일종의 ==양파==로 생각하라고 아이디어를 주었다.

```
        requests
           |
           v
+----- layer_three -----+
| +---- layer_two ----+ |
| | +-- layer_one --+ | |
| | |               | | |
| | |    handler    | | |
| | |               | | |
| | +-- layer_one --+ | |
| +---- layer_two ----+ |
+----- layer_three -----+
           |
           v
        responses
```
