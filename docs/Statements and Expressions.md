---
description:
aliases: 
tags: 
date created: Monday, February 27th 2023, 1:43:07 am
date modified: Monday, February 27th 2023, 6:20:45 pm
created: 2023-02-27T01:43:07
updated: 2023-07-15T21:33:03
title: Statements and Expressions
---

> Statement(구문)은 값을 리턴하지 않는 명령이고 Expression(표현식)은 값을 리턴하는 명령이다.

$$
Statement \subset Expression
$$

- Statements
	- 함수 정의 `fn function(...){...}` 은 Statement이다.
	- `let` 구문은 Statement이다. => C는 변수 할당 결과가 또 다른 변수에 들어갈 수 있다는 차이점이 있다. `x = y = 6` 

	```rust
	let x = (let y = 6); // Compile Error
	```

	- 문장 마지막에 붙는 세미콜론 `;` 는 해당 문장을 Statement로 만든다.

- Expressions
	- 할당연산자 `` = `` 의 우측값은 Expression이다. 예를 들어 `let y = 6;` Statement 중 `` 6 `` 은 Expression이다. 
	- 함수를 호출하는 것은 Expression이다.
	- 매크로를 호출하는 것은 Expression이다.
	- `{}` 는 Expression이다.

	```rust
	let x = { let y = 6; y }; // Ok x <- 6
	```

	- if 문은 Expression이다.
	- 세미콜론 `;` 이 붙지 않은 문장은 Expression이다.
	- `break` 뒤에 오는 표현은 Expression이다. 따라서, 반복문은 Expression이 될 수 있다.

	```rust
	let mut counter = 10;
	let something = loop {
		counter += 1;
		if counter == 10 {
			break counter * 2; // loop가 `counter * 2` 를 평가하게 된다.
		}
	}; // 이 loop가 expression임을 알려준다.
	```

	- 
___
https://rust-book.cs.brown.edu/ch03-03-how-functions-work.html#statements-and-expressions
