---
description:
aliases: 
tags: programming/rust 
created: 2023-03-08T18:37:38
updated: 2023-07-11T15:21:07
title: Returning a Reference to the Stack
---
[Fixing Ownership Errors](https://rust-book.cs.brown.edu/ch04-03-fixing-ownership-errors.html)

아래의 코드는 컴파일 에러를 일으킨다. s는 해당 함수가 리턴할때 끝나는 아주 짧은 라이프타임을 가지고 있지만, 리턴타입은 s의 레퍼런스를 계속 가리키고 있기 때문이다. 따라서 borrow checker에 의해 컴파일 단계에서 에러를 일으키게 된다.
```rust
fn return_a_string() -> &String {
	let s = String::from("Hello world");
	&s
}
```

문제를 해결하는데엔 여러가지 방법이 존재한다.

1. Ownership 자체를 리턴하는 방법: 가장 단순한 해결책이지만 스코프 안에서 메모리 할당이 일어난다.
```rust
fn return_a_string() -> String {
	let s = String::from("Hello world");
	s
}
```

2. 소스코드에 문자열 리터럴을 박아넣기: 수정이 필요없는 경우에 한에서 아주 유용하다. 마치 cpp의 `constexpr` 키워드를 보는 것 같군
```rust
fn return_a_string() -> &'static str {
	"Hello, World!"
}
// OR
static STRING: &str = "Hello, World!";
```

3. [Reference-counted pointer](https://doc.rust-lang.org/std/rc/index.html) 를 적극적으로 활용하기: 마치 cpp의 `shared_pointer` 처럼 동작하는 것 같은데, 레퍼런스 카운트가 0가 되는 순간 메모리가 해제된다. 
```rust
use std::rc::Rc;
fn return_a_string() -> Rc<String> {
	let s = Rc::new(String::from("Hello, World"));
	Rc::clone(&s)
}
```

4. api를 사용하는 쪽에서 메모리 공간을 마련하도록 만들기 
> Have the caller provide a "slot" to put the string using the mutable reference
```rust
fn return_a_string(output: &mut String) {
	output.replace_range(.., "Hello, World");
}
```
