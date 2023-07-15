---
description:
aliases: 
tags: 
created: 2023-03-29T20:40:17
updated: 2023-07-15T21:33:03
title: Ord is total order, PartialOrd is partial order
---
- [실험을 진행한 코드](https://github.com/ChoiWheatley/my-first-rust/blob/c55e7e848ccc2eefb0838b4ba44a3d7e86922f07/self_ref/src/step_006_self_ref_cmp.rs)
- [Ord 문서](https://doc.rust-lang.org/std/cmp/trait.Ord.html)
- [PartialOrd 문서](https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html)
- [전순서, 부분순서 namu](https://namu.wiki/w/%EC%88%9C%EC%84%9C%20%EA%B4%80%EA%B3%84)  
다음 모듈은 `Ord` 트레이트의 속성에 대하여 실험을 진행하는 코드이다.  
복합 구조체 `Node`에 대하여 노드의 어떤 속성을 기준으로 비교를 진행하는지  
확인한 결과, 모든 멤버들에 대한 비교를 진행하는 것으로 확인됐다.

[Ord](https://doc.rust-lang.org/std/cmp/trait.Ord.html)의 첫번째 줄에서  
이미 다음 글귀를 확인할 수 있었다. (좀 일찍 볼걸..)

> Trait for types that form a **total order**  
> 전순서 집합을 형성하는 트레이트

```rust
mod tutorial {
    use std::collections::BTreeSet;
    #[derive(PartialEq, Eq, PartialOrd, Ord, Debug)]
    struct Node {
        name: String,
        id: u32,
    }
    #[cfg(test)]
    mod tests {
        use super::*;
        #[test]
        fn run() {
            let mut holder: BTreeSet<&Node> = BTreeSet::new();
            let nodes = [
                Node {
                    name: "n1".to_owned(),
                    id: 1,
                },
                Node {
                    name: "n2".to_owned(),
                    id: 2,
                },
                Node {
                    name: "n3".to_owned(),
                    id: 3,
                },
            ];
            nodes.iter().for_each(|each| {
                holder.insert(each);
            });

            assert_eq!(3, holder.len());

            // what if we insert same `name` node in holder?
            let dup1 = Node {
                name: "n1".to_owned(),
                id: 123124,
            };
            holder.insert(&dup1);

            assert_eq!(4, holder.len()); // Ok

            // what if we insert same `id` node in holder?
            let dup2 = Node {
                name: "dup2".to_owned(),
                id: 2,
            };
            holder.insert(&dup2);

            assert_eq!(5, holder.len());

            // what if we insert same both `name` and `id` in holder?
            let dup3 = Node {
                name: "n3".to_owned(),
                id: 3,
            };
            holder.insert(&dup3);

            assert_eq!(5, holder.len()); // didn't inserted!!
        }
    }
}

```

# 전순서 집합이 아닌 커스텀 Ord도 만들 수 있다.

https://doc.rust-lang.org/std/cmp/trait.Ord.html#how-can-i-implement-ord  
전순서가 아닌 비교 트레이트는 `PartialOrd` 이다. `Ord`를 구현하기 위해선 우선 `PartialOrd` + `Eq` ( + `PartialEq` ) 를 만족하여야 한다. 아래 코드는 height에 대해서만 비교를 수행하는 커스텀 `Ord`를 보여주고 있다.

```rust
use std::cmp::Ordering;

#[derive(Eq)]
struct Person {
    id: u32,
    name: String,
    height: u32,
}

impl Ord for Person {
    fn cmp(&self, other: &Self) -> Ordering {
        self.height.cmp(&other.height)
    }
}

impl PartialOrd for Person {
    fn partial_cmp(&self, other: &Self) -> Option<Ordering> {
        Some(self.cmp(other))
    }
}

impl PartialEq for Person {
    fn eq(&self, other: &Self) -> bool {
        self.height == other.height
    }
}
```

비교가능 여부도 알 수 있는 PartialOrd
---
여기에서 `PartialOrd` 인터페이스 `partial_cmp`의 리턴형이 옵션인 이유에 대해 알게 되었다. 다음 [clippy - neg_cmp_on_partial_ord](https://rust-lang.github.io/rust-clippy/master/index.html#neg_cmp_op_on_partial_ord) 문서에서 `partial_cmp`는 세가지 기본 비교연산(`Eq`, `Less`, `Greater`) 외에도 한 가지 연산을 더 수행할 수 있는데, 바로 **비교가능**인지 여부이다. 만약 두 연산자가 말이 안되는 경우( divide by zero )로 인해 비교자체가 불가능한 경우 `None`을 리턴하고, 이외엔 `Some(Ordering)` 을 리턴한다.
