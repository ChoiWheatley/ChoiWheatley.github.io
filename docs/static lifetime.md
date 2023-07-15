---
description:
aliases: 
tags: programming/rust 
created: 2023-03-08T16:45:02
updated: 2023-07-15T21:33:03
title: static lifetime
---
https://doc.rust-lang.org/rust-by-example/scope/lifetime/static_lifetime.html

`'static` 키워드가 붙은 레퍼런스는 레퍼런스가 가리키는 값이 런타임동안 살아있다는 것을 뜻한다. (전역변수?) 바이너리에 read-only 영역에 보관된다고 한다. (elf 파일 기준 `.text` 영역에 보관된다는 소리겠지?)

- Make a constant with the `static` declaration
- Make a `string` literal which has type: `&'static str`.

```rust
// Make a constant with 'static lifetime
static NUM: i32 = 18;

// Returns a reference to NUM where its 'static
// lifetime is coerced to that of the input argument
fn coerce_static<'a>(_: &'a i32) -> &'a i32 {
	&NUM
}

fn main() {
	{
		// Make a string literal and print it:
		let static_string = "I'm in read-only memory";
		println!("static_string: {}", static_string);

		// When static_string goes out of scope, the reference
		// can no longer be used, but the data remains in the
		// binary.
	}
	{
		// Make an integer to use for corece_static:
		let lifetime_num = 9;

		// Coerce NUM to lifetime of lifetime_num
		let coerced_static = coerce_static(&lifetime_num);

		println!("coerced_static: {}". coerced_static);
	}

	println!("NUM: {} stays accessible!", NUM);
}
```

[코드실행결과](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=ca360b973383f41fce86f81ab64ebb54)

`'` 이렇게 single quote 하나만 앞에 찍혀있는 건 타입이 아니라 라이프타임이었나보다. 그래서 coerce 코드를 보면 어떤 제네릭 변수가 있을때 이 변수의 라이프타임을 `NUM` 에게 강제하겠다는 뜻처럼 보인다.
