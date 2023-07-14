---
description:
aliases: 
tags: 
created: 2023-04-01T10:38:03
updated: 2023-07-11T15:21:08
title: immutable reference is copyable, mutable reference is not copyable
---
- [forum talking about immutable references implement Copy](https://users.rust-lang.org/t/do-immutable-references-implement-copy/70075)
- [Copy marker, not trait](https://doc.rust-lang.org/std/marker/trait.Copy.html#impl-Copy-73)
- [When can't my type be Copy?](https://doc.rust-lang.org/std/marker/trait.Copy.html#when-cant-my-type-be-copy)
	- Some types can't be copied safely. For example, copying `&mut T` would create an aliased mutable reference.
- [Linked List의 IterMut을 구현하다가 밟은 레고조각 - Learning Rust with Entirely Too Many Linked Lists](https://rust-unofficial.github.io/too-many-lists/second-iter-mut.html)
- 