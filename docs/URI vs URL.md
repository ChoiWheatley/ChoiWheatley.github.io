---
aliases: 
tags: 
description:
created: 2023-07-05T14:07:38
updated: 2023-07-11T15:21:07
title: URI vs URL
---

[uri vs url](https://blog.hubspot.com/website/uri-vs-url)
- URI, Uniform Resource Identifier
- URL, Uniform Resource Locator

한 마디로 정리하자면, URI $\supset$ URL이고 URI에 프로토콜 (http 또는 ftp, etc)과 도메인/서브도메인(hubspot, naver, etc.) 붙으면 URL이 된다.

URI의 뜻을 직역하면 "통합 자원 식별자"인데, 우리는 이 식별자에 지목할 것이다. 식별자란 무엇인가? 비슷하게 생겨먹은 수많은 자원 중에서 내가 원하는 것을 정확히 지칭할 수 있는 고유값을 의미한다. 예를 들면 주민번호가 될 것이고, 시리얼 넘버가 될 것이다. URI는 다음 요소를 가지고 있다.

[syntax components](https://datatracker.ietf.org/doc/html/rfc3986#section-3)
1. Scheme: 모든 URI의 시작을 알리는 요소. 
2. Authority: 해당 이름에 대한 권한이 있는, 그래서 그 권한당사자에게 URI를 대신 전달할 수 있는 구체적인 이름을 뜻한다. 주소값이 될 수도 있고, 이름이 될 수도 있고, 끝에 포트번호 또는 유저정보가 붙을 수도 있다. authority는 `//`로 시작하고 `/`, `?`, `#` 혹은 그 자체로 끝난다.
3. Path: 계층구조로 구조화된 패스는 리소스를 식별하는 역할을 한다.

URL은 URI의 목적을 달성할 수 있다. 다만, 이름 Locator라는 이름에 걸맞게 해당 리소스가 어디에 있는지 알려주려는 목적을 띄고있다.

