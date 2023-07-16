---
description:
aliases: 
tags: 
created: 2023-05-22T21:41:50
updated: 2023-07-15T21:33:03
title: RESTful API란 무엇인가 -- web, resources, osi 7layers
---

# API란?

Application Programming Interface의 약자로, SW간에 상호작용을 위한 제안된 함수, 핸들 목록을 의미한다. 인터페이스이기 때문에 구현체를 알지 않아도 인터페이스만 안다면 해당 기술을 별 무리없이 사용할 수 있게되어 아주 강력한 추상화를 이루어 낼 수 있다.

# REST API란?

[yalco - posting](https://www.yalco.kr/23_rest_api/) | [IBM - What is a REST API?](https://www.ibm.com/topics/rest-apis) | [hubspot.com - URI VS URL](https://blog.hubspot.com/website/uri-vs-url) | [그런 REST API로 괜찮은가](https://youtu.be/RP_f5dMoHFc)

> 하이퍼텍스트를 포함한 Self-descriptive한 메시지의 Uniform Interface를 통해 **리소스**에 접근하는 API

REpresentation State Transfer의 약자로, 무슨 뜻인지 모르겠으니까 몇가지 특징을 짚어보자.

1. 웹상에 주고받는 통신규약으로 가장 유명한 HTTP 프로토콜이 있다.
2. 하지만 이것만으로 인터페이스를 설계하자, 아주 중구남방이 되어버렸다. 대표적으로 URI의 구분자를 일관성 없이 사용한 문제가 발생.
3. REST API는 URL만 읽어도 대충 서버에 어떤 데이터를 주고받는지 알 수 있도록 HTTP상에 제약을 추가했다. (HTTP를 사용하는 것은 동일)
	1. **Uniform Interface**: 모든 리소스는 오직 하나의 URI를 갖는다.
	2. **Client-Server Decoupling**: 클라이언트와 서버간 유일한 통신수단으로 HTTP 프로토콜을 활용한 REST를 강제한다. 
	3. **Statelessness**: 모든 요청은 그 자체로 구조와 식별자를 모두 갖추고 있어야 한다. 서버사이드 세션이 없다는 의미이며, 클라이언트가 전송한 요청을 어떤 방식으로든 저장하는 것을 금지한다.
	4. **Cacheability**: 다만, 자원만큼은 캐싱이 가능하게끔 설계하여 이것으로 하여금 확장성을 높일 수 있게 만든다.
	5. **Layered System Architecture**: 레이어 아키텍처는 목적지에 도달하는 일부 과정을 추상화한 개념이다. 따라서 REST API상에 서버-클라이언트 통신과정에서 미들웨어나 중간자에 대한 이해 없이도 가능하도록 해야한다.
	6. **Code On Demand(optional)**: 서버의 응답은 실행가능한 코드조각을 담아 보낼 수도 있다. 이것을 온-디맨드라고 부른다. 가장 대표적으로, .js파일을 보내 클라이언트가 실행할 수 있다.

## Web?

인터넷이 발전하던 1990년대, 엔티티 간에 데이터를 공유할 수 있는 방법으로 고안. 

- HTML
	- 데이터의 표현형식. 하이퍼텍스트라서 다른 HTML이나 리소스를 참조할 수 있다.
- URI
	- 리소스의 식별자. 방대한 인터넷에서 문서나 파일, 사진 등을 찾아내기 위한 계층구조로 이루어져있다.
- HTTP
	- 하이퍼텍스트인 HTML파일을 효과적으로 전송 및 수신하기 위해 고안된 규칙.

## What are Resources?

- html 문서
- photo
- txt 파일
- 등등

## 그런 REST API로 괜찮은가 스크랩

REST api가 구현하기 까다로운 이유는 다름아닌 Uniform Interface의 제약조건 때문이다. 단순히 유일한 URI를 만드는 것은 쉽다. 다만 메시지 자체가 **self-descriptive**한지 여부와 **HATEOAS**(헤이티오스)를 준수하는지 여부가 우리의 발목을 잡는다.  
![[fwer23.png]]
- self-descriptive message  
	자기스스로 설명하는 메시지란, HTML 형식을 보면 바로 알 수 있다. 우리가 a태그를 모르면 어딜 찾아가서 확인해야하는지 알고있듯이, SW가 주고받는 문서의 내용을 추론할 수 있어야 한다.

	엥? 그러면 우리도 `hello`같은 태그 만들어서 어떻게 작동할지 명시하면 되는거 아니야? => TRUE. 그래서 위 영상에서 하고자 하는 말은 심지어 JSON파일일지라도 그 key값이 의미하는 바를 추론할 수 있도록 요청 헤더에 [IANA](https://www.iana.org/assignments/media-types/media-types.xhtml)의 참조를 추가하면 자가설명이 가능해진다. 사실 JSON 파싱하는 방법 자체에 대해서도 IANA에 적혀있다고...

- HATEOAS  
	쉽게 말해 링크를 타고 **들어가는** 일을 앱 단위의 상태변화로 볼 수 있다. HTML은 애초에 하이퍼텍스트이기 때문에 상태변화가 자연스럽게 느껴지지만, 일반 텍스트 에디터를 보면 아무리 그 안에 링크가 적혀있어도 에디터의 상태가 변하지는 않잖아.

REST API는 웹에서 지원하고 있는 HTTP + REST 수준을 정확히 그대로 따라야 하는가?  
![[h2o3iruw89efy.png]]

그렇다면 모든 원격 API들은 REST API 제약조건을 준수하여야 하는가?  
![[9sd93sdsdAS 1.png]]

JSON은 RESTful한가?  
![[naver d2 - Day1, 2-2. 그런 REST API로 괜찮은가 [RP_f5dMoHFc - 1314x739 - 38m34s].png]]

## URI? URL?

[[URI vs URL]]

# OSI 7 Layers

[네트워크 top down aproach overview pdf](https://1drv.ms/b/s!AgE-lhMulmhxg5UUghoFBc0ThAoxNQ?e=Jtr77T)


![[스크린샷 2023-05-26 21.27.51.png]]
