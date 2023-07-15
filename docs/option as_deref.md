---
description:
aliases: 
tags: 
created: 2023-04-01T00:37:59
updated: 2023-07-15T21:33:03
title: option as_deref
---
- https://doc.rust-lang.org/std/option/enum.Option.html#method.as_deref
- Converts from `Option<T>` or `&Option<T>` to `Option<&T::Target>`.
- Creates a new one with a reference to the original one, additionally coercing the contents via [[Deref trait]]  
이거 없었으면 한 번 옵션을 unwrap 하거나 map 하는 과정을 겪어야 하는데 내부 타입만을 빌린 타입으로 바꿔주니까 유용하네~

```rust
struct Node(i32);
let original = Node(1);
let option_box_node: Option<Box<Node>> = Some(Box::new(original));
let option_borrowed: Option<&Node> = option_box_node.as_ref().map(|e| &**e); // same as below
let option_borrowed: Option<&Node> = option_box_node.as_ref().map::<&Node, _>(|e| e); // same as below
let option_borrowed: Option<&Node> = option_box_node.as_deref();
```

`as_ref()` 와의 차이점을 명확하게 인지하고 가자. 얘는 `&` 를 붙여주는 녀석이다.
