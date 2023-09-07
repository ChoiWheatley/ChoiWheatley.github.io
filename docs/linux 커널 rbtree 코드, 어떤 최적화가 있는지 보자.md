---
aliases: 
tags: 
description:
title: linux 커널 rbtree 코드, 어떤 최적화가 있는지 보자
created: 2023-09-07T13:55:57
updated: 2023-09-07T14:18:39
---
- parent: [[이진검색트리 red black tree|RBTree]]
___

## README

[[week 04 {swjungle} {Red Black Tree}]]의 마지막 날, 코치님께서 rbtree color를 int형으로 선언한 것을 최적화 하는 방법에 대해서 이야기를 해주셨다. 이 문서는 어떻게 구조체 안에 선언된 노드의 컬러 (RED, BLACK) 정보를 별도의 바이트를 쓰지 않고 메모리 공간을 아낄 수 있는지에 대한 조사를 작성할 예정이다.

## INDEX

- [linux/include/linux/rbtree_types.h {GH}](https://github.com/torvalds/linux/blob/7ba2090ca64ea1aa435744884124387db1fac70f/include/linux/rbtree_types.h)

`rb_node` 선언코드가 여기에 들어있다. 그런데 struct 선언 뒤에 `__attribute__((aligned(sizeof(long))))` 이런것도 있다. 컴파일러 (GCC)에게 추가적인 정보를 넘겨 최적화를 유도하는 기능으로 이해했다. [gcc/Type-Attributes.html](https://gcc.gnu.org/onlinedocs/gcc-3.3/gcc/Type-Attributes.html) 문서를 읽어보면, 해당 attribute를 통해 임의의 구조체를 정렬할 때 기본값인 워드 사이즈가 아닌 주어진 바이트만큼 정렬하도록 강요할 수 있다고 한다.

```c
struct rb_node {
	unsigned long  __rb_parent_color;
	struct rb_node *rb_right;
	struct rb_node *rb_left;
} __attribute__((aligned(sizeof(long))));
/* The alignment might seem pointless, but allegedly CRIS needs it */
```

- [linux/include/linux/rbtree.h {GH}](https://github.com/torvalds/linux/blob/7ba2090ca64ea1aa435744884124387db1fac70f/include/linux/rbtree.h)
- [linux/lib/rbtree.c {GH}](https://github.com/torvalds/linux/blob/7ba2090ca64ea1aa435744884124387db1fac70f/lib/rbtree.c)
