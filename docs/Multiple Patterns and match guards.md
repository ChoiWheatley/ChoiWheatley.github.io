---
description:
aliases: 
tags: 
created: 2023-03-27T23:25:45
updated: 2023-07-11T15:21:08
title: Multiple Patterns and match guards
---
- https://rust-book.cs.brown.edu/ch18-03-pattern-syntax.html#multiple-patterns
- https://rust-book.cs.brown.edu/ch18-03-pattern-syntax.html#extra-conditionals-with-match-guards

Multiple patterns with `|`
```rust
let x = 1;
match x { 
	1 | 2 => println!("one or two"), 
	3 => println!("three"), 
	_ => println!("anything"), 
}
```

with `..=` syntax
```rust
let x = 5;

match x {
	1..=5 => println!("one through five"),
	_ => println!("something else"),
}

```

tuple을 분해하는 건 이미 알고있지
```rust
let x = (10, 'A');

match x {
	(num, alpha) => println!("num: {}, alpha: {}", num, alpha),
	_ => println!("something else"),
}
```

하지만 struct를 분해할 수도 있다는 건 몰랐을걸?
```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 0, y: 7 };

    let Point { x: a, y: b } = p;
    assert_eq!(0, a);
    assert_eq!(7, b);
    match p { 
	    Point { x, y: 0 } => println!("On the x axis at {}", x), 
	    Point { x: 0, y } => println!("On the y axis at {}", y), 
	    Point { x, y } => println!("On neither axis: ({}, {})", x, y), 
	}
}
```

match guard를 사용하면 추가적인 조건을 걸 수가 있지롱
```rust
let num = Some(4);

match num {
	Some(x) if x % 2 == 0 => println!("The number {} is even", x),
	Some(x) => println!("The number {} is odd", x),
	None => (),
}
```

`@` bindings 를 사용하면 match guard 안에서 변수를 할당할 수 있지렁
```rust
enum Message {
	Hello {id: i32},
}
let msg = Message::Hello{id: 5};
match msg {
	Message::Hello { id } if id >=3 && id <= 7 => 
		println!("Found an id in range: {}", id),
	Message::Hello { id } => println!("Found some other id: {}", id),
}
```
이렇게 쓰지말고 아래처럼 쓰면 좀 더 깔끔하쥬
```rust
enum Message {
	Hello {id: i32},
}
let msg = Message::Hello{id: 5};
match msg {
	Message::Hello {id: _id @ 3..=7} =>
		println!("Found an id in range: {}", _id),
	Message::Hello { id } => println!("Found some other id: {}", id),
}
```