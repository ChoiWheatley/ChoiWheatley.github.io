---
description:
aliases: 
tags: 
created: 2023-04-10T15:12:58
updated: 2023-07-11T15:21:08
title: exception safety with rust
---
- https://doc.rust-lang.org/nightly/nomicon/exception-safety.html
- https://rust-unofficial.github.io/too-many-lists/sixth-panics.html
- [exception-safe code - CppCon](https://youtu.be/W7fIy_54y-w) 안전한 코드, 안전한 예외처리에 대한 방법론과 생각의 기술은 언어-specific 하지 않아 가져옴. 
- TL;DR **Do or Never**
- DB 시간에 배웠던 규칙과 매우 닮았다. 범용 프로그래밍 언어는 자체적으로 커밋을 지원하지는 않지만 (스위프트 제외) 커밋을 하는 것처럼 코드를 짜는 유용한 방법들에 대한 방법론은 꽤나 다양하다.
- 직관적으로 생각해봤을 때, 실패할 가능성이 있는 코드를 코드블럭 맨 처음 또는 맨 마지막에 두는 것이 실용적이라고 할 수 있다. 

nomicon 예시를 그대로 들고왔다. 다음 `Vec::push_all`은 슬라이스 길이를 가지고 미리 `reserve`를 해서 불필요한 len 체크를 없앴다. 하지만 이 코드는 exception-safe 하지 않다고 한다. 
```rust
impl<T: Clone> Vec<T> {
    fn push_all(&mut self, to_push: &[T]) {
        self.reserve(to_push.len());
        unsafe {
            // can't overflow because we just reserved this
            self.set_len(self.len() + to_push.len());

            for (i, x) in to_push.iter().enumerate() {
                self.ptr().add(i).write(x.clone());
            }
        }
    }
}
```

`clone` 메서드는 본질적으로 위험하다. 왜냐고? `Vec` 입장에서는 저 클론이 어떻게 이루어지는지 모른다. 사용자가 디폴트 할당자를 사용했을지, 커스텀 할당자를 사용했을지 모른다는 것이다. 만약에 중간에 클론을 하다가 panic이 발생한다면, 프로그램은 stack-unwinding을 수행할 것이고, 나머지 것들이 정리가 안 된 상태로 블럭을 빠져나가게 된다.
블럭을 빠져나가는 것 자체가 문제가 되는 것은 아니다. 하지만 우리는 클론을 하기 전에 벡터의 len을 바꾸지 않았나? 이것이 바로 문제가 된다. uninitialized 된 벡터의 원소를 사용자가 접근할 여지를 남겼기 때문이다. 따라서 이것은 Undefined Behavior이다.

그렇다면, 위 코드를 어떻게 바꾸어야 할까? 저자는 몇가지 방법을 제안한다.
1. `set_len`을 매 `write` 이후에 한 번씩 수행한다.
2. 루프가 끝난 뒤에야 `set_len`을 호출한다.