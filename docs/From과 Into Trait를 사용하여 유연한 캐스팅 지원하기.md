---
description:
aliases: 
tags: 
created: 2023-03-25T09:43:25
updated: 2023-07-15T21:33:04
title: From과 Into Trait를 사용하여 유연한 캐스팅 지원하기
---
- https://www.geeksforgeeks.org/rust-from-and-into-traits/

에러타입의 종류가 많은 enum을 리턴해야 하는 함수가 있다면 From 트레이트를 사용하여 코드를 간결하게 만들 수 있다고 한다.

### 기본 용법

```rust
use std::conver::From;
fn main() {
	let _my_str = "Hello, World!"; // 'static &str
	let _my_string = String::from(_my_str);
}
```

### 커스텀 From

이건 그냥 생성자잖아

```rust
use std::conver::From;

#[derive(Debug)]
struct GFG {
	year: i32,
}

impl From<i32> for GFG {
	fn from(item: i32) -> Self {
		GFG { year: item }
	}
}

fn main() {
	let num = GFG::from(2023);
	println!("learning Rust from {:?}", num);
}
```

### into helper function

`From` 트레이트를 구현한 객체에 한하여 구체적인 타입을 명시한 변수에 할당될 객체를 자동으로 찾아서 `from`을 호출하게 해준다.

```rust
// ...
fn main() {
	let num = 2023;
	let gfg: GFG = num.into();
	println!("learning Rust from {:?}", num);
}
```

### Error handling with enum and From trait

트레이트로 아예 타입변환에 대한 메서드를 정의해버렸기 때문에 early return (error propagation operator) `?` 연산자를 사용하여 해당 구문을 실패했을 경우 자동으로 일치하는 타입의 `from` 메서드를 호출하여 에러를 리턴하게 될 것이다.

```rust
enum CliError{
	IoError(io::Error),
	ParseError(num::ParseIntError),
}
impl From<io::Error> for CliError{
	fn from(error: io::Error) -> Self {
		CliError::IoError(error)
	}
}
impl From<num::ParseIntError> for CliError{
	fn from(error: num::ParseIntError) -> Self {
		CliError::ParseError(error)
	}
}
fn open_and_parse_file(file_name: &str) -> Result<i32, CliError> {
	let mut contents = fs::read_to_string(&file_name)?;
	let num: i32 = contents.trim().parse()?;
	Ok(num)
}
```
