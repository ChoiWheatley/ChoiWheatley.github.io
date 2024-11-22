---
aliases: 
tags: question/unresolved
description:
title: 프론트엔드에서의 form과 백엔드에서의 form fields는 독립적이어야 하나 {drf, django}
created: 2023-07-28T15:20:21
updated: 2023-07-28T15:23:01
---
- links
	- [[3차 프로젝트, ChatGPT를 이용한 챗봇 애플리케이션 - estsoft {Django, DRF}]]
	- [[0014.1.1 drf {django rest framework} 😴]]
	- [[0014.1 Django 🎈]]
___

# Q.

form도 독립적이어야 하나? form이 어느정도는 DB 스키마와 관련이 있을 수도 있는데, 그러면 백엔드에서 form fields를 제공하는 엔드포인트가 필요한거임?프론트가 스스로 form을 만드는 것과 백엔드가 form fields를 정의하는 것의 차이가 궁금하다.

곰곰히 생각해보니 DB 스키마가 프론트에 영향을 미치는 것이야말로 강한 결합도를 의미한다. 따라서 프론트는 _어떤_ form field가 필요한지 결정하면 안되지 않을까?
