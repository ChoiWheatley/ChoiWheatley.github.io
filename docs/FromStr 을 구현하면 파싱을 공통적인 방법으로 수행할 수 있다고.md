---
description:
aliases: 
tags: 
created: 2023-03-25T10:16:38
updated: 2023-07-15T21:33:04
title: FromStr 을 구현하면 파싱을 공통적인 방법으로 수행할 수 있다고
---
- https://doc.rust-lang.org/std/str/trait.FromStr.html

예를 들어 우리가 `"(1,2)"` 를 제공하면 `Point` 객체를 알아서 파싱해서 리턴해주는 함수를 만든다고 가정하자. 

```rust
struct Point { x: i32, y: i32, }
struct ParsePointError;
fn parse_to_point(s: &str) -> Result<Point, ParsePointError> {
	// ...
}
```

뭐 이렇게 해도 세상이 멸망하는 정도는 아니지만 std 의 다양한 응용을 활용할 수 없다는 문제가 있다. 따라서 문자열로부터 객체를 파싱해야 할 일이 있을 땐 `FromStr` 트레이트를 구현하도록 하자.

```rust
use std::str::FromStr;

#[derive(Debug, PartialEq)]
struct Point { x: i32, y: i32, }

#[derive(Debug, PartialEq, Eq)]
struct ParsePointError;

impl FromStr for Point {
	type Err = ParsePointError;

	fn from_str(s: &str) -> Result<Self, Self::Err> {
		let (x, y) = s
			.strip_prefix('(')
			.and_then(|s| s.strip_suffix(')')
			.and_then(|s| s.split_once(','))
			.ok_or(ParsePointError)?;
		let x_fromstr: i32 = x.parse().map_err(|_| ParsePointError)?;
		let y_fromstr: i32 = y.parse().map_err(|_| ParsePointError)?;
		Ok(Point {x: x_fromstr, y: y_fromstr})
	}
}
```

사용할 수 있는 편의 함수들이 이젠 널리고 널렸다. 다음 코드를 보자

```rust
fn main() {
	let expected = Ok(Point {x: 1, y: 2});
	let fromstr = "(1,2)";
	// Explicit call
	assert_eq!(Point::from_str(&fromstr), expected);
	// Implicit calls
	assert_eq!(fromstr.parse(), expected);
	assert_eq!(fromstr.parse::<Point>(), expected);
```
