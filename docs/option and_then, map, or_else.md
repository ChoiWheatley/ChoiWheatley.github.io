---
description:
aliases: 
tags: 
created: 2023-04-02T15:57:46
updated: 2023-07-15T21:33:03
title: option and_then, map, or_else
---
- https://riptutorial.com/rust/example/9668/using-option-with-map-and-and-then
- `Option::map`과 성질이 거의 비슷하지만 `and_then`은 클로저가 `Option`을 리턴해도 알아서 **flatten** 해준다는 점에 차이가 있다. 무슨 말이냐고?

```rust
let something: Option<i32> = Some(100);
let mapped_ok: Option<i32> = something.map(|i| i / 10);
// divided by 0 문제를 해소해 주는 `checked_div`는 Option을 리턴한다. 따라서 `Option`이 두 번 씌워지게 되는데, 이는 우리가 원하는 방식이 아니다.
let mapped_no: Option<Option<i32>> = something.map(|i| i.checked_div(0));
let solution: Option<i32> = something.and_then(|i| i.checked_div(0));
```

- 덕분에 `Option` 내부의 값을 체이닝 하여 단계적으로 바꿔나갈 수 있게 되었다.

```rust
struct Node {
	next: Option<Node>,
	elem: i32,
	// bunch of gunks
}
// peek next
let next_element: Option<Node> = node
	.and_then(|node| node.next)
	.map(|node| node.elem);
```

- [or_else](https://doc.rust-lang.org/std/option/enum.Option.html#method.or_else)는 체이닝 도중에 발생하는 예외(Option이니까 당연히 None)에 대하여 클로저를 호출하고 클로저의 리턴을 그대로 리턴한다. 뒤 체인은 신경도 쓰지 않는다!
- 
