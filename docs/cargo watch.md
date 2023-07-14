---
description:
aliases: 
tags: 
created: 2023-04-18T18:19:33
updated: 2023-07-11T15:21:09
title: cargo watch
---
아래의 코드는 다음 의미를 갖는다. 매번 `cargo run` 하지 말고 저장할 때마다 
1. `--quiet` : 조용히(cargo watch 자체의 소음만 제거)
2. `--clear` : 매번 화면을 초기화 하여 
3. `--watch src/` : src 폴더를 감시하며
4. `--exec run` : `cargo run` 을 매번 호출한다.
```shell
cargo watch -q -c -w src/ -x run
```
