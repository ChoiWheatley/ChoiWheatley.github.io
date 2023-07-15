---
description:
aliases: 
tags: 
created: 2023-03-29T21:00:56
updated: 2023-07-15T21:33:03
title: rust에서 c++와 같은 custom comparator를 정의할 수 있을까요
---
- https://stackoverflow.com/questions/34028324/how-do-i-use-a-custom-comparator-function-with-btreeset#34037318
- 정답! False
- 현재로썬 커스텀 비교자를 정의할 수 없다. 따라서 별도의 Ord를 정의하는 래퍼 구조체를 정의하여야 한다...

```rust
struct Wrapper(Wrapped);
impl Ord<Wrapped> for Wrapper {
	fn cmp(&self, other: &Self) -> Ordering {}
}
impl From<Wrapped> for Wrapper {
	fn from(value: Wrapped) -> Self {}
}
```
