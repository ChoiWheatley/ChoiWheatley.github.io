---
description:
aliases: 
tags: 
created: 2023-04-04T22:23:55
updated: 2023-07-15T21:33:04
title: is there a way to iterate through the values of an enum
---
- https://stackoverflow.com/questions/21371534/in-rust-is-there-a-way-to-iterate-through-the-values-of-an-enum
- 스태틱 배열을 직접 선언하면 된다.

```rust
pub enum Direction {
    North,
    South,
    East,
    West,
}

impl Direction {
    pub fn iterator() -> Iter<'static, Direction> {
        static DIRECTIONS: [Direction; 4] = [North, South, East, West];
        DIRECTIONS.iter()
    }
}
```
