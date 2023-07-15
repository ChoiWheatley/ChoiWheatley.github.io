---
description:
aliases: 
tags: 
created: 2023-04-10T22:01:54
updated: 2023-07-15T21:30:20
title: Debug trait and DebugList builder and DebugStruct builder etc
---
- [TMLL](https://rust-unofficial.github.io/too-many-lists/sixth-random-bits.html) 막바지에 들어가면서 얻어갈 것이 많아 기쁘다.
- `Debug` 트레잇을 구현하는 코드가 다음과 같았다.

```rust
impl<T: Debug> Debug for LinkedList<T> {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        f.debug_list().entries(self).finish()
    }
}
```

- 나는 처음에 iterator를 가지고 순회하면서 출력할 줄 알았는데, 저건 좀 신박했다. 먼저 `debug_list` 메서드를 찾아보자.
- [doc](https://doc.rust-lang.org/std/fmt/trait.Debug.html)
	- 프로그래머 기준에 출력을 제공하는 트레잇이다. 어떤 타입에 대한 pretty-print를 할 수 있다.
- 아래는 `debug_struct` 메서드에 대한 스니펫이다. 엄청 신기한데

```rust
struct Point {
    x: i32,
    y: i32,
}

impl fmt::Debug for Point {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        f.debug_struct("Point")
         .field("x", &self.x)
         .field("y", &self.y)
         .finish()
    }
}
```

- 와, 이거 뿐만 아니라 `debug_tuple`, `debug_list`, `debug_set`, `debug_map`도 있다! 헬퍼가 엄청나게 많다!!!
- 그래서 알아보고자 하는 `debug_list`에 대한 스니펫은 아래와 같다. 
	- `entries` 안에 인자로 보니까 `Debug` 트레잇을 구현한 *런타임* 객체면 다 되는 것 같다.

```rust
struct Foo(Vec<i32>);

impl fmt::Debug for Foo {
    fn fmt(&self, fmt: &mut fmt::Formatter) -> fmt::Result {
        fmt.debug_list().entries(self.0.iter()).finish()
    }
}
/// function signature of `entry`
pub fn entry(&mut self, entry: &dyn Debug) -> &mut DebugList<'a, 'b>
```

- [?] 왜 모든 헬퍼함수들에는 `&dyn Debug`가 붙었을까? dynamic dispatch를 해야만 하는 이유가 있을 것 같지 않은데...
