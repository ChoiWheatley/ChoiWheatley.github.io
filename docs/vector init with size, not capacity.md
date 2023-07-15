---
description:
aliases: 
tags: 
created: 2023-03-28T11:28:17
updated: 2023-07-15T21:33:03
title: vector init with size, not capacity
---
평화로운 어느날, 최승현은 코드를 이렇게 짰다.

```rust
let mut v = Vec::with_capacity(size);
v.resize(size, 0);
```

`cargo clippy`를 친 최승현은 소스라치게 놀라고 말았다. 더 좋은 방법이 있었기 때문이다!

```
  --> src/bitset.rs:23:9
   |
22 |         let mut v = Vec::with_capacity(size);
   |                     ------------------------ help: consider replace allocation with: `vec![0; size]`
23 |         v.resize(size, 0);
   |         ^^^^^^^^^^^^^^^^^
   |
```
