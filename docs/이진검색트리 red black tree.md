---
description:
title: 이진검색트리 red black tree
created: 2023-02-13T06:16:26
categories: 
 - 알고리즘
 - 이진검색트리 
 - red black tree
aliases: 
 - red black tree
 - binary search tree
 - BST
tags: [" algo/tree algo/graph algo/datastructure  ", algo/tree, algo/graph, algo/datastructure]
date created: Monday, February 13th 2023, 6:16:26 am
date modified: Monday, February 27th 2023, 6:20:45 pm
updated: 2023-09-04T01:18:33
---
parent link: [[0011 Algorithms ♾️]]

---

## 이진검색트리 red black tree

상태: Not started  
태그: datastructure, graph, tree

Ranged search에 강력한 성능을 보이는 트리 자료구조를 직접 구현해보자. 이진탐색트리를 먼저 구현하고 스스로 균형을 조절하는 RB-tree (Red-black tree) 트리를 만들어 보겠다.

- [이진 탐색 트리 - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/%EC%9D%B4%EC%A7%84_%ED%83%90%EC%83%89_%ED%8A%B8%EB%A6%AC)
- [레드-블랙 트리 - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/%EB%A0%88%EB%93%9C-%EB%B8%94%EB%9E%99_%ED%8A%B8%EB%A6%AC)

## INDEX

- 참고하기 좋은 자료들
	- CSLR Chapter 13 Red-Black Trees
	- C로 쓴 자료구조론 - Horowitz, Sahni Anderson-Freed, 이석호 역 10.3 Red-Black Trees
	- [msambol/dsa/trees/red_black_tree.py {GH}](https://github.com/msambol/dsa/blob/master/trees/red_black_tree.py)
	- [redblack tree visualization tool](https://www.cs.usfca.edu/~galles/visualization/RedBlack.html)
- keywords
	- left rotate
	- right rotate
	- insert
		- insert_fixup
			- case1) Z's uncle is red
			- case2) Z's uncle is black and Z's parent, grandparent form a triangle
			- case3) Z's uncle is black and Z's parent, grandparent form a line
	- deletion
		- transplant (u, v)
			- case1) u's parent is None
			- case2) u is u's parent's left child
			- case3) u is u's parent's right child
		- delete (x)
			- case1) left child is NIL
			- case2) right child is NIL
			- case3) both of children are not NIL
		- delete_fixup (x)
			- let w x's sibling
			- case1) w is red
			- case2) w is black, w's both children are black
			- case3) w is black, w's left child is red, right child is black
			- case 4) w is black, w's right child is red

## ADT

```yaml
ADT RBTree:
	objects: 공백이거나 루트노드, 왼쪽 노드, 오른쪽 노드로 구성되는 노드들의 유한집합
	functions: 
		모든 t, t1, t2 ∈ RBTree에 대하여
		- RBTree new_tree(): 비어있는 트리 생성
		- delete_tree(tree): 트리의 메모리 반환
		- tree_insert(tree, key): tree에 key 추가
		- elem_p tree_find(tree, key): 트리 안에 key 탐색 후 노드의 포인터 또는 NULL 반환
		- tree_erase(tree, ptr): 트리 내부의 ptr로 지정된 노드 삭제 후 메모리 반환
		- elem_p tree_min(tree): 트리 중 최소 값을 가진 노드의 포인터 반환
		- elem_p tree_max(tree): 트리 중 최대 값을 가진 노드의 포인터 반환
		- tree_to_array(tree, array_out, n): 
			- 트리의 내용을 key 순서대로 주어진 array로 변환. 
			- array의 크기는 n으로 주어지며, tree의 크기가 n보다 큰 경우 순서대로 n개 까지만 반환
			- array의 메모리 공간은 이 함수를 부르는 쪽에서 준비하고 그 크기를 세번째 인자로 알려줍니다.
```

## Helper Functions

- `insert_case(u, parent, grandparent, uncle) -> LLr | LRr | RLr | RRr | LLb | LRb | RLb | RRb`
- `rotate_left(x)`
- `rotate_right(x)`
- `transplant(u, v)`
	- u자리를 v로 대치하고 u 서브트리를 연결해제한다.
- `travel_bfs(T, callback)`
	- bfs 순회를 한다.
- `travel_dfs(T, callback)`
	- dfs 순회를 한다.
- `subtree_min(u)`
	- u 서브트리의 최솟값에 해당하는 노드를 리턴한다.
- `subtree_max(u)`
	- u 서브트리의 최댓값에 해당하는 노드를 리턴한다.
- 

## Delete

### Example

<iframe title="Red-black trees in 8 minutes — Deletions" src="https://www.youtube.com/embed/lU99loSvD8s?feature=oembed" height="113" width="200" allowfullscreen="" allow="fullscreen" style="aspect-ratio: 1.76991 / 1; width: 100%; height: 100%;"></iframe> 
다음 영상의 예시를 그대로 따라가봤다. msambol 깃허브 페이지에도 파이썬 코드가 있으니 참고바란다.

![[rbtree_delete_example_01.jpg]]

![[rbtree_delete_example_02.jpg]]

### summary

총 두 가지 스텝이 있다. 먼저 delete의 설명부터. 솔직히 case 1,2는 어렵지 않다. RBtree 제약조건을 위반하지 않으면서 단순히 NIL이 아닌 자식을 끌어올리기만 하면 그만이거든.

진짜 문제는 z의 자식이 모두 존재할 경우이다. z를 사전순으로 가장 가까운 노드인 y로 대치해야 하는데, 그 과정에서 black의 손실이 일어나기 때문이다. y를 z의 오른쪽 서브트리의 최솟값이라고 했을때, y의 왼쪽 자식은 두말할 것 없이 NIL이고, y의 오른쪽 자식을 x라고 해보자. x가 심지어 NIL일 지라도 일단 눈 한번 딱 감고 transplant를 해보자. 그러면 x는 y.p의 자식이 될 것이고 x.p는 y.p가 될 것이다. 상황에 따라서 NIL.p를 설정할 수도 있지만 괜찮다.

![[rbtree_delete_summary_01.jpg]]

빼버린 노드가 black 노드라면 곧 심각한 문제에 처하게 된다. black이 빠진 서브트리의 rank가 기존과 달리 1 줄어들었기 때문이다. 우리는 fixup을 수행할 때 딱 한가지만 머리에 들고 출발하면 된다.

> subtree 'x'의 rank를 1 높히자. 다만 다른 서브트리가 RBTree 제약조건을 어기지 않는 선에서.

![[rbtree_delete_summary_02.jpg]]

### Fixup

![[rbtree_delete_fixup_type01.jpg]]

![[rbtree_delete_fixup_type02.jpg]]

![[rbtree_delete_fixup_type03.jpg]]

![[rbtree_delete_fixup_type04.jpg]]
