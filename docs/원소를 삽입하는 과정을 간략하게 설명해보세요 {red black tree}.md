---
aliases: 
tags: 
description:
title: 원소를 삽입하는 과정을 간략하게 설명해보세요 {red black tree}
created: 2024-01-10T23:38:57
updated: 2024-01-10T23:46:00
---

 - [[0012 Career 💼]]
 - [[이진검색트리 red black tree#Insert]]
---
삽입할 위치를 찾아 BST 검색을 수행합니다. 리프 노드에 도달한 경우 새 Red 노드를 삽입합니다. 이제 "red node는 연속하지 않는다"는 제약조건을 어긴 경우 트리를 회전합니다. 다양한 케이스가 존재하지만  핵심 로직은 삽입된 노드부터 루트까지 각각의 서브트리가 RBTree 제약조건을 만족시키게끔 만드는 것입니다.
