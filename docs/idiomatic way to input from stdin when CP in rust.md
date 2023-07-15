---
description:
aliases: 
tags: 
created: 2023-04-07T00:53:26
updated: 2023-07-15T21:33:04
title: idiomatic way to input from stdin when CP in rust
---
CP 종특, 숫자만 입력으로 받고 whitespace로 구분을 하기 때문에 미리 iterator에 map을 하여 모든 라인에 대하여 자동으로 parse하게끔 만들어 놓았다. `flat_map`인 이유는 빈 문자열을 *flatten* 해주어 또 한 번의 `unwrap`의 필요를 제거했기 때문이다.

아래 코드는 매 라인별 하나의 정수형이 들어올 때를 위한 코드이다.

```rust
let mut lines = io::stdin()
	.lines()
	.flat_map(|res_str| res_str.map(|s| s.parse::<usize>().expect("parse error")));
```

아래 코드는 매 라인에 하나 이상의 정수형이 들어올 때를 위한 코드이다. [`split_ascii_whitespace`](https://doc.rust-lang.org/std/str/struct.SplitAsciiWhitespace.html) 또는 [`split_whitespace`](https://doc.rust-lang.org/std/str/struct.SplitWhitespace.html) 는 임의의 whitespace를 기준으로 이터레이트 할 수 있게 만들어준다.

```rust
let mut lines = io::stdin().lines().map(|res_str| {
	res_str.map(|s| {
		s.split_ascii_whitespace()
			.map(|i| i.parse::<i32>().expect("parse error"))
			.collect::<Vec<_>>()
	})
});
```
