---
description:
aliases: 
tags: 
date created: Thursday, February 23rd 2023, 10:20:20 pm
date modified: Monday, February 27th 2023, 6:20:45 pm
created: 2023-02-23T22:20:20
updated: 2023-07-15T21:33:03
title: trim
---
string을 파싱하기 전엔 무조건 트림을 해야 하는구나...

```rust
let mut guess = String::new(); io::stdin()
	.read_line(&mut guess) 
	.expect("Failed to read line"); 
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```
