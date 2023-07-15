---
description:
aliases: 
tags: 
created: 2023-04-10T22:57:18
updated: 2023-07-15T21:33:04
title: Hasher에게 먹이 주기 feed Hasher
---
- https://rust-unofficial.github.io/too-many-lists/sixth-random-bits.html
- https://doc.rust-lang.org/std/hash/index.html
- 해시까지 떠먹여 주는 우리 갓갓 저자
- 기본적으로 `Hash`는 derivable이다. 하지만 커스텀 해시가 필요한 경우가 있을 때 구현해 줄 수 있다.
- 아주 간단한 메커니즘만 알면 된다. `Hash` 트레잇을 구현한 모든 타입은 `hash` 메서드를 호출할 수 있는데, 인자에 feeder를 정의할 수 있다. 내가 어떤 타입에 대한 해시값을 만들고 싶다면 `Hasher` 타입 변수를 하나 생성하고 연관된 모든 value들에 대한 `hash` 메서드에 인자로 담금질을 하면 완성이다.

```rust
pub trait Hash {
	fn hash<H>(&self, state: &mut H)
	where H: Hasher;
}
```

예시

```rust
struct Person {
    id: u32,
    name: String,
    phone: u64,
}

impl Hash for Person {
    fn hash<H: Hasher>(&self, state: &mut H) {
        self.id.hash(state);
        self.phone.hash(state);
    }
}
```

# Disclaimer: [Hash and Eq](https://doc.rust-lang.org/std/hash/trait.Hash.html#hash-and-eq)

두 변수의 해시값이 같으면 반드시 `eq`가 참이어야 한다.

# Disclaimer: [Prefix Collisions](https://doc.rust-lang.org/std/hash/trait.Hash.html#prefix-collisions)

예를 들어 다음과 같은 경우에 Prefix Collision이 발생한다.

```rust
["he", "llo"].hash(state1);
["hello"].hash(state2);
assert_eq!(state1, state2);
```

따라서, 해시를 저시기 하기 위하여 `len` 값을 같이 끼워넣어야 충돌을 막을 수 있다고 한다.

그런고로, 우리의 [[Too Many Linked Lists|TMLL]] 프로젝트에서 구현한 `Hash` 내용물을 잠깐 보여주자면 다음과 같이 생겼다.

```rust
impl<T: Hash> Hash for LinkedList<T> {
    fn hash<H: std::hash::Hasher>(&self, state: &mut H) {
        self.len().hash(state); // ⬅️ HERE!!
        for item in self {
            item.hash(state);
        }
    }
}
```
