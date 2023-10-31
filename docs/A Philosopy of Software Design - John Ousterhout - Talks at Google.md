---
aliases: 
tags:
  - scrap
description: 
title: A Philosopy of Software Design - John Ousterhout - Talks at Google
created: 2023-10-31T14:06:32
updated: 2023-10-31T14:21:11
---
- [[0010 Programming 👩‍💻]]
---

## README

내용도 많고 주제도 많으니까 그때그때 들으면서 정리해보자.

## 클래스의 확장은 어디까지 염두해야할까?

<https://www.youtube.com/watch?v=bmSAYlu0NcY&t=986s>

**question**

evolution of class, 아직 구현되지 않은 코드들을 미래에 추가하는 것이 괜찮을까?

**answer**

물론 내일을 편하게 살기위해 클래스를 쓰고 가까운 미래에 코드를 클래스에 넣는 일은 일어날 수 있기는 하지만 클래스 자체가 선언하는 순간 그대로 굳혀져 버리기 때문에 (specialized) 인터페이스를 수정하는 데엔 큰 책임이 따른다. 그래서 섣불리 너무 많은 인터페이스를 식별하려고 노력할 필요는 없다. 
