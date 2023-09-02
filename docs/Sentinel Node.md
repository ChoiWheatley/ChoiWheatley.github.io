---
aliases: 
tags: scrap 
description:
title: Sentinel Node
created: 2023-09-02T23:40:39
updated: 2023-09-02T23:41:28
---
test-rbtree.c 의 코드 내용을 보면 `#ifdef SENTINEL` 이라는 코드가 있습니다.관련 내용을 찾아보다 보니 NULL을 사용하는 대신 Sentinel Node를 사용해서 linked list, binary search tree 등을 구현할 수 있다는 걸 배웠습니다.  
Sentinel node를 사용하면서 생기는 이점이 저한테는 매력적으로 다가와서 한번쯤 고려해보시면 좋을 것 같습니다.

- Sentinel Node란?: [https://en.wikipedia.org/wiki/Sentinel_node](https://en.wikipedia.org/wiki/Sentinel_node)
- Sentinel Node를 사용했을 때 생기는 이점: [https://stackoverflow.com/questions/5384358/how-does-a-sentinel-node-offer-benefits-over-null](https://stackoverflow.com/questions/5384358/how-does-a-sentinel-node-offer-benefits-over-null)
- Sentinel Node를 사용해 double-linked-list 를 구현하기: [http://web.eecs.utk.edu/~bvanderz/teaching/cs140Fa10/notes/Dllists/](http://web.eecs.utk.edu/~bvanderz/teaching/cs140Fa10/notes/Dllists/)
