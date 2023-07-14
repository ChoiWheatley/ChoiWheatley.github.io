---
description:
aliases: 
tags: 
created: 2023-05-29T16:53:21
updated: 2023-07-11T15:21:07
title: POST 요청을 기가막하게 사용하기 { django }
---
- https://youtu.be/sMqDJovFO-Y?t=5853
- 위의 방법은 아주 단순하다. `blog/<int:pk>/like`리소스를 클라이언트가 POST 요청하면 서버는 해당 아티클에 대한 몇가지 판단 (유저유효성 검사, 유저가 이미 해당 아티클에 좋아요를 눌렀는지 여부 등)을 진행한 뒤 Article 모델의 `liked`상태를 변경한다.
- 그리고 나서 아주 기가막히게도, `redirect`를 한다...! 
	- [[redirect { django }]]