---
description:
aliases: 
tags: 
date created: Monday, February 27th 2023, 1:43:07 am
date modified: Monday, February 27th 2023, 6:20:45 pm
created: 2023-02-27T01:43:07
updated: 2023-07-11T15:21:08
title: loop label
---
기본적으로 `break` 나 `continue` 구문은 가장 안쪽의 반복문에 연관이 있지만 이것을 명시적으로 바꿀 수 있다.
```rust
'L1: loop {
	'L2: loop {
		'L3: loop{
			break 'L1;
		}
	}
}
```