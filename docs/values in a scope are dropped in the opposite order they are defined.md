---
description:
aliases: 
tags: 
created: 2023-04-02T17:01:18
updated: 2023-07-11T15:21:07
title: values in a scope are dropped in the opposite order they are defined
---
[[Too Many Linked Lists]] 에서 ch03인 persistent list의 `Drop` 트레이트를 구현하자 오류가 나지 않았던 테스트 코드에서 변수 수명 에러가 나타났다.

```rust
#[test]
fn iter() {
	let mut list = List::new();
	let queue = "Hello, world!".chars().collect::<Vec<_>>();
	queue.iter().for_each(|c| { // queue does not live long enough
		list = list.prepend(c);
	});
	let answer_iter = queue.iter().rev();
	let list_iter = list.iter();
	assert!(list_iter
		.zip(answer_iter)
		.all(|(&list, answer)| list == answer));
}
```

그 말은 즉슨, `queue`에 의존하는 `list`가 `queue`가 드롭이 된 뒤에도 살아있기 때문에 발생한 문제인 것이었다. drop은 별도로 명시하지 않는 한 블럭의 마지막에 가서 일어날 것이고, drop은 정의된 반대 순서로 진행되기 때문에 `list` 를 `queue` 보다 먼저 정의한 이 코드에 따라서 컴파일러는 `queue`를 먼저 삭제하려고 할 것이고, 이로 인해 변수 수명 이슈가 발생한 것이다!!!

수정은 매우 EZ하다. 단지 `queue`정의문을 `list` 보다 위에 써주면 해결된다.

```
   error[E0597]: `queue` does not live long enough
   --> too_many_linked_lists/src/ch03_persistent_stack.rs:124:9
    |
124 |         queue.iter().for_each(|c| {
    |         ^^^^^^^^^^^^ borrowed value does not live long enough
...
134 |     }
    |     -
    |     |
    |     `queue` dropped here while still borrowed
    |     borrow might be used here, when `list` is dropped and runs the `Drop` code for type `ch03_persistent_stack::List`
    |
    = note: values in a scope are dropped in the opposite order they are defined
```

