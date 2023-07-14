---
description:
aliases: 
tags: 
date created: Monday, February 27th 2023, 1:43:07 am
date modified: Monday, February 27th 2023, 6:20:45 pm
created: 2023-02-27T01:43:07
updated: 2023-07-11T15:21:08
title: let 변수 선언만 해도 되나요
---
가능합니다. 다음 문장을 보면
```rust
let x;
if true {
	x = 1;
} else {
	x = 2;
}
```
은 cpp의 입장에서 볼 땐 불가능하지만, 러스트의 입장에서는 가능합니다. 컴파일러가 잔머리를 굴려서 x에 오직 한 번만 할당이 되는지 파악했고, 동시에 가능한 할당구문이 동일한 타입인지도 파악했기 때문입니다.

# 하지만,

아래의 방식이 좀 더 구체적이고 직관적이죠. 
```rust
let x = if true { 1 } else { 2 };
```
___
https://rust-lang.github.io/rust-clippy/master/index.html#needless_late_init