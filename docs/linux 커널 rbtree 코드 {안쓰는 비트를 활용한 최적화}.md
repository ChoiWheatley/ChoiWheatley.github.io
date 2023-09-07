---
aliases: 
tags: 
description:
title: linux 커널 rbtree 코드 {안쓰는 비트를 활용한 최적화}
created: 2023-09-07T13:55:57
updated: 2023-09-07T21:21:20
---
- parent: [[이진검색트리 red black tree|RBTree]]
- [linux/include/linux/rbtree_types.h {GH}](https://github.com/torvalds/linux/blob/7ba2090ca64ea1aa435744884124387db1fac70f/include/linux/rbtree_types.h)
- [linux/include/linux/rbtree.h {GH}](https://github.com/torvalds/linux/blob/7ba2090ca64ea1aa435744884124387db1fac70f/include/linux/rbtree.h)
- [linux/lib/rbtree.c {GH}](https://github.com/torvalds/linux/blob/7ba2090ca64ea1aa435744884124387db1fac70f/lib/rbtree.c)
___

## README

[[week 04 {swjungle} {Red Black Tree}]]의 마지막 날, 코치님께서 rbtree color를 int형으로 선언한 것을 최적화 하는 방법에 대해서 이야기를 해주셨다. 이 문서는 어떻게 구조체 안에 선언된 노드의 컬러 (RED, BLACK) 정보를 별도의 바이트를 쓰지 않고 메모리 공간을 아낄 수 있는지에 대한 조사를 작성할 예정이다.

## rbtree_types.h

### `__attribute__((aligned(sizeof(long))))`인가?

`rb_node` 선언코드가 여기에 들어있다. 그런데 struct 선언 뒤에 `__attribute__((aligned(sizeof(long))))` 이런것도 있다. 컴파일러 (GCC)에게 추가적인 정보를 넘겨 최적화를 유도하는 기능으로 이해했다. [gcc/Type-Attributes.html](https://gcc.gnu.org/onlinedocs/gcc-3.3/gcc/Type-Attributes.html) 문서를 읽어보면, 해당 attribute를 통해 임의의 구조체를 정렬할 때 기본값인 워드 사이즈가 아닌 주어진 바이트만큼 정렬하도록 강요할 수 있다고 한다.

```c
struct rb_node {
	unsigned long  __rb_parent_color;
	struct rb_node *rb_right;
	struct rb_node *rb_left;
} __attribute__((aligned(sizeof(long))));
/* The alignment might seem pointless, but allegedly CRIS needs it */
```

그래서 다음 실습을 해보자. 세 assert 모두 통과한다. `short`가 2바이트이므로 정렬을 수행할 때 2바이트를 기준으로 정렬한다. 하지만 명시적으로 8바이트를 기준으로 정렬하도록 강제했기에, 두번째 assert에서 8이 나오는 것을 알 수 있다.

```c
#include <stdio.h>
#include <assert.h>

struct K {short f[3];};
struct S {short f[3];} __attribute__((aligned (8)));
struct P {int f[3];} __attribute__((aligned (8)));

int main(int argc, char *argv[]) {
  assert(sizeof(struct K) == 6);
  assert(sizeof(struct S) == 8);
  assert(sizeof(struct P) == 16);
  return 0;
}
```

그래서 다시 `struct rb_node`를 보자. `sizeof(long)`은 일반 64비트 컴퓨터 기준으로 8바이트이므로 `struct rb_node`는 8바이트 포인터 두개에 long형 `__rb_parent_color`까지 하여 총 8 * 3 = 24바이트를 차지할 것이다. 그런데 `sizeof(long)` 또한 8인데, 결국 `__attribute__`를 사용한다 쳐도 8 * 3 = 24바이트 구조체를 할당한다는 사실은 변하지 않는다. 그저 컴퓨터 아키텍처에 따라 다른 정렬의 구조체에 대응하기 위한 코드로 보인다.

### `__rb_parent_color`에 숨겨진 의미라도?

[rbtree_types.h](https://github.com/torvalds/linux/blob/7ba2090ca64ea1aa435744884124387db1fac70f/include/linux/rbtree_types.h#L5-L10) 와 [rbtree_augmented.h](https://github.com/torvalds/linux/blob/7ba2090ca64ea1aa435744884124387db1fac70f/tools/include/linux/rbtree_augmented.h#L147-L157) 파일을 훑어보다가 흥미로운 매크로 함수를 발견했습니다.

```c
// rbtree_types.h
struct rb_node {
        unsigned long  __rb_parent_color;
        struct rb_node *rb_right;
        struct rb_node *rb_left;
} __attribute__((aligned(sizeof(long))));
/* The alignment might seem pointless, but allegedly CRIS needs it */
```

```c
/// rbtree_augmented.h
#define	RB_RED		0
#define	RB_BLACK	1

#define __rb_parent(pc)    ((struct rb_node *)(pc & ~3))

#define __rb_color(pc)     ((pc) & 1)
#define __rb_is_black(pc)  __rb_color(pc)
#define __rb_is_red(pc)    (!__rb_color(pc))
#define rb_color(rb)       __rb_color((rb)->__rb_parent_color)
#define rb_is_red(rb)      __rb_is_red((rb)->__rb_parent_color)
#define rb_is_black(rb)    __rb_is_black((rb)->__rb_parent_color)
```

`__rb_parent_color`는 비록 하나의 멤버변수이지만 포인터 연산의 특징을 영리하게 활용했습니다. 모든 `rb_node`에 대한 포인터는 4 또는 8바이트 이하로 쪼개질 수 없습니다. 따라서 첫 두 비트는 항상 0입니다. 

parent 포인터가 필요할 땐 `__rb_parent` 매크로 함수를 이용하여 첫 두 비트를 0으로 만들어버리는 비트 마스킹을 취한 후 포인터로 캐스팅을 합니다. 반대로 color가 필요할 땐 `__rb_color` 매크로 함수를 이용하여 첫 비트가 0인지, 1인지만 봅니다.

## struct rb_node에 key가 없다

key가 없으면 도대체 뭘 저장하려고 rbtree를 만든거지?

[linux/Documentation/core-api/rbtree.rst](https://github.com/torvalds/linux/blob/7ba2090ca64ea1aa435744884124387db1fac70f/Documentation/core-api/rbtree.rst)에 따르면, 내부 IO 스케줄러가 요청을 추적하기 위해 사용하고 ext3 파일 시스템또한 디렉토리 항목을 추적하기 위해 사용합니다. 가상 메모리 영역(VMA), 계층 토큰 버킷(?), [epoll file descriptor](https://man7.org/linux/man-pages/man7/epoll.7.html) 등에서 활용된다고 합니다.

그리고 `rb_node` 자체에 데이터를 저장하지 않고 데이터를 저장하는 struct를 따로 만들어 그 안에 `rb_node` 멤버를 가지게 만드는 것으로 사용자가 직접 타입을 확장할 수 있게 했습니다.
