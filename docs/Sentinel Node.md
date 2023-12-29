---
aliases: 
tags: scrap 
description:
title: Sentinel Node
created: 2023-09-02T23:40:39
updated: 2023-09-03T14:24:20
---
test-rbtree.c 의 코드 내용을 보면 `#ifdef SENTINEL` 이라는 코드가 있습니다.관련 내용을 찾아보다 보니 NULL을 사용하는 대신 Sentinel Node를 사용해서 linked list, binary search tree 등을 구현할 수 있다는 걸 배웠습니다.  
Sentinel node를 사용하면서 생기는 이점이 저한테는 매력적으로 다가와서 한번쯤 고려해보시면 좋을 것 같습니다.

- Sentinel Node란?: [https://en.wikipedia.org/wiki/Sentinel_node](https://en.wikipedia.org/wiki/Sentinel_node)
- Sentinel Node를 사용했을 때 생기는 이점: [https://stackoverflow.com/questions/5384358/how-does-a-sentinel-node-offer-benefits-over-null](https://stackoverflow.com/questions/5384358/how-does-a-sentinel-node-offer-benefits-over-null)
- Sentinel Node를 사용해 double-linked-list 를 구현하기: [http://web.eecs.utk.edu/~bvanderz/teaching/cs140Fa10/notes/Dllists/](http://web.eecs.utk.edu/~bvanderz/teaching/cs140Fa10/notes/Dllists/)

> Why use the sentinel? The sentinel makes it possible to treat a NIL child of a node x as an ordinary node whose parent is x. An alternative design would use a distinct sentinel node for each NIL in the tree, so that the parent of each NIL is well defined. That approach needlessly wastes space, however. Instead, just the one sentinel T.nil represents all the NILs—all leaves and the root’s parent. The values of the attributes p, left, right, and key of the sentinel are immaterial.  
> Cormen, Thomas H.; Leiserson, Charles E.; Rivest, Ronald L.; Stein, Clifford. Introduction to Algorithms, fourth edition (p. 332). MIT Press. Kindle Edition. 

별도의 값(예를 들면 -1)을 둔 노드를 우리 나름대로 외부노드(리프노드)라고 정의할 수도 있겠지만 공간낭비가 있을 수 있다. 따라서 클래스 변수 `NIL`을 두어 그 위치로 포인팅 하는 기법을 센티넬이라고 부른다.
