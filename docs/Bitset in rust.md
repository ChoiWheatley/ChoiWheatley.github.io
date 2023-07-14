---
description:
aliases: 
tags: 
created: 2023-03-26T21:27:41
updated: 2023-07-11T15:21:09
title: Bitset in rust
---
- moved to notion https://choiwheatley.notion.site/Bitset-in-rust-56ebc5cb130a4fdfb7cdecf9f78b232e
- parent [[0013 Rust 🦀]]
___
[[러스트로 game of life 구현하기]]를 진행하면서 enum 벡터를 최적화 할 수 있는 방법으로 어차피 bool 벡터인 셈이므로 비트셋을 사용하는 방법이 좋겠다고 판단하여 구현을 시작하였다.

C++에서 비트셋을 만들었던 기억이 있으므로 딱 두 가지만 기억하면 된다.
- `bin_number = index / (size_of(element_type) * 8)` 
- `bit_number = index % (size_of(element_type) * 8)` 

두 파라메터를 구하는 함수 `bin_no` 와 `inner_idx` 를 작성하였다. 이거 근데 [[러스트에서 인라인 정의]] 못하나? => 간접적으로 외부에서 호출되지 않는 private 함수에 대해선 굳이 인라인 할 필요 없다.

```rust
use std::mem::size_of;

fn bin_no(idx: usize) -> usize {
	idx / (size_of::<usize>() * 8)
}

fn inner_idx(idx: usize) -> usize {
	idx % (size_of::<usize>() * 8)
}
```

`Bitset` 타입과 생성자를 정의해보자. 우리가 만들 비트셋은 초기 크기가 고정되어있다. 아직은 배열 크기를 제네릭하게 만드는 방법을 모르니 벡터를 쓰자. [[generic array in rust]]

```rust
pub struct Bitset {
	bits: Vec<usize>,
}

impl Bitset {
	/// creates [false; size] like bitset
	pub fn with_size(size: usize) -> Self {
		let size = bin_no(size) + 1; // 혹시 모르니 여유롭게
		let mut v = Vec::with_capacity(size); // size 아닙니다, capacity 입니다.
		v.resize(size, 0);
		Bitset {bits: v}
	}

	/// indices의 각 원소들의 인덱스가 true인 새 Bitset를 반환한다.
	pub fn from_indices(indices: &[usize]) -> Self {
		let size = bin_no(indices.len()) + 1;
		let mut v = Vec::with_capacity(size);
		v.resize(size, 0);	
		for i in indices.iter().cloned() {
			*v.get_mut(bin_no(i)).expect("Index out of bounds") |= 1 << inner_idx(i);
		}
		Bitset { bits: v }
	}
}
```

다음은 인덱싱을 활용하여 get, set을 하는 메서드를 정의해보자

```rust
impl Bitset {
	pub fn get(&self, idx: usize) -> bool {
		self.bits.get(bin_no(idx))
			.expect("Index Out of Bounds") >> inner_idx(idx) & 0b01 == 1
	}
    pub fn set(&mut self, idx: usize) {
        *self.bits.get_mut(bin_no(idx))
	        .expect("Index Out of Bounds") |= 1 << inner_idx(idx);
    }
    pub fn reset(&mut self, idx: usize) {
        *self.bits.get_mut(bin_no(idx))
	        .expect("Index Out of Bounds") &= !(1 << inner_idx(idx));
    }
    pub fn set_to(&mut self, idx: usize, to: bool) {
        if to {
            self.set(idx);
        } else {
            self.reset(idx);
        }
    }
```

아래는 비트셋 관련 테스트이다. [[러스트로 game of life 구현하기]] 할때 디버깅 용으로 간단하게 테스트 코드를 작성했다. 이때 러스트 모듈 구조에 대한 이해를 덤으로 얻어갔다.
1. 유닛 테스트는 테스트 하려는 모듈과 같은 파일 안에서 수행한다.
2. 테스트 모듈파일 안에 `#[cfg(test)] mod tests{}` 블럭 안에서 테스트 코드를 작성하면 된다. -- [[자식 모듈은 부모 모듈의 private 요소들을 자유롭게 접근할 수 있다]]는 점 기억하고.

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_bin_no() {
        for i in 0..=128 {
            assert_eq!(i / 32, bin_no(i), "i : {}", i);
        }
    }

    #[test]
    fn test_init() {
        let bs = Bitset::with_size(32);
        assert_eq!(2, bs.bits.len());
        for i in 0..32 {
            assert_eq!(false, bs.get(i), "in index {i}");
        }
        let bs = Bitset::with_size(128);
        assert_eq!(5, bs.bits.len());
        for i in 0..128 {
            assert_eq!(false, bs.get(i), "in index {}", i);
        }
    }

    #[test]
    fn test_set_get() {
        let size = 128;
        let mut bs = Bitset::with_size(size);
        for i in 0..size {
            bs.set(i);
            assert!(bs.get(i));
        }
        for i in (0..size).rev() {
            bs.reset(i);
            assert!(!bs.get(i));
        }
    }
}

```

___
[fixedbitset](https://crates.io/crates/fixedbitset) crate를 사용하는 방법도 있다. 하지만 굳이? 