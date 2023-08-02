---
aliases: 
tags: 
description:
title: Token based login {drf} {ormi-project-3}
created: 2023-08-02T15:31:27
updated: 2023-08-02T15:38:23
---
DRF는 Stateless 원칙을 지키기 위해 Token based Authentication을 제공한다. (물론 세션방식도 있긴 함) 나는 `is_authenticated`만 만족시키면 되기 때문에 뭐 특별히 커스텀 권한을 만들 필요는 없다. 하지만 아직까지 JWT를 사용하여 요청의 유저를 식별하는 방법에 대해서 잘 알지 못하겠다.

우선 [[DRF에서 인증기능 만들기 {drf}]]에서 공부한 simple jwt 문서를 더 읽어보는 것으로 출발하자.
