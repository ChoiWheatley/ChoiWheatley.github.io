---
description:
aliases: 
tags: 
created: 2023-03-27T16:31:39
updated: 2023-07-11T15:20:17
title: 자식 모듈은 부모 모듈의 private 요소들을 자유롭게 접근할 수 있다
---
자식 모듈은 부모 모듈의 private 요소들을 자유롭게 접근할 수 있다.

테스트 코드를 예로 들면, run 함수는 현재 어떤 모듈 `foo.rs` 안에 속해있다고 가정하자. 그러면 `test_run` 함수는 전체 path가 `crate::foo::tests::test_run` 일 것이다. 이때 `tests` 모듈은 `foo` 모듈의 private fn인 `run` 을 호출할 수 있을까? YES. 

> Rust chose to have the module system function this way so that hiding inner implementation details is the default. [link](https://rust-book.cs.brown.edu/ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html)

```rust
// src/foo.rs
fn run() { }

#[cfg(test)]
mod tests {
	use super::*;
	#[test]
	fn test_run() {
		run();
	}
}
```

다만 상대주소 (마치 디렉토리 시스템에서 `..` 과 같이)를 적용하기 위해서 `super` 키워드를 사용하여야 한다. `super` 사용을 줄이기 위해 `use` 키워드를 사용할 수 있고.