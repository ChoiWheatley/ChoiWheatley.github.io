---
description:
aliases: 
tags: 
created: 2023-04-08T11:47:24
updated: 2023-07-11T15:21:08
title: malloc like rust feature
---
- https://doc.rust-lang.org/std/boxed/struct.Box.html#method.into_raw
- TL;DR `Box::into_raw(Box::new(...))`
- 아시다시피 힙영역에 메모리를 요청하는 방법으로는 `Box::new()` 가 가장 유명하다. 그냥 구조체 생성을 해버리면 힙 영역이 아니라 스택 영역에 할당된다는거 잊지말자.
- 그런데 [[Too Many Linked Lists]] 의 unsafe queue를 구현하면서 raw pointer를 사용하려고 하는데 다음과 같이 코드를 작성했다.
```rust
struct List<T> {
	head: *mut Node<T>,
	tail: *mut Node<T>,
}
struct Node<T> {
	elem: T,
	next: *mut Node<T>,
}
//...
pub fn push(&mut self, elem: T) {
	let new_tail = Node { elem, next: std::ptr::null_mut() };
	// ...
}
```

이렇게 쓰면 코딩인생 망함. Node를 생성하는 블럭을 벗어나면 `new_tail`에 대한 데이터가 drop이 되는데 거기에 연결된 `head`나 `tail`이나 전부 dangling pointer가 되는 것이다! 얏호! 

어쨌든 결국 힙영역에 메모리를 요청해야 하고, `Box`의 도움을 받아야 한다. 
```rust
impl List<T> {
pub fn push(&mut self, elem: T) {
	let new_tail = Box::new(Node {elem, next: std::ptr::null_ptr() };
	self.head = ???
	self.tail = ???
}
}
```

그런데 `Box` 타입은 필요 이상으로 안전하다. 따라서 이걸 포인터로 바꿔주어야 한다. 이때 사용하는 방법이 바로 `Box::into_raw()` 이다.

```rust
let new_tail = Box::into_raw(
	Box::new(
		Node { elem, std::ptr::null_mut() }
	));
```

이게뭔 `malloc`같은 소리인지 모르겠는데, 러스트에선 이렇게 써야 한다...

마지막으로 해당 타입을 drop 하거나 move할 필요가 있을 땐 포인터를 deref만 하면 안된다! 반드시 Box로 다시 래핑하여서 객체를 줘야만 한다.

```rust
// Re-wrap into Box
let ret = Box::from_raw(self.head);

self.head = ret.next;

if self.head.is_null() {
	// empty tail again!
	self.tail = null_mut();
}

Some(ret.elem)
```
