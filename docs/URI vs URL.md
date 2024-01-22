---
aliases: 
tags: 
description:
created: 2023-07-05T14:07:38
updated: 2024-01-22T14:51:00
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

## 2024-01-22 추가

scrapped from [inpa.tistory.com / WEB URL URI 차이](https://inpa.tistory.com/entry/WEB-🌐-URL-URI-차이)

#### 목차

1.  [URL / URI / URN 차이점](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-URI-%EC%B0%A8%EC%9D%B4#url_/_uri_/_urn_%EC%B0%A8%EC%9D%B4%EC%A0%90)
    1.  [URI / URL / URN 정의](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-URI-%EC%B0%A8%EC%9D%B4#uri_/_url_/_urn_%EC%A0%95%EC%9D%98)
        1.  [URI (Uniform Resource Identifier)](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-URI-%EC%B0%A8%EC%9D%B4#uri_uniform_resource_identifier)
        2.  [URL (Uniform Resource Locator)](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-URI-%EC%B0%A8%EC%9D%B4#url_uniform_resource_locator)
        3.  [URN (Uniform Resource Name)](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-URI-%EC%B0%A8%EC%9D%B4#urn_uniform_resource_name)
    2.  [URI / URL / URN 구분하기](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-URI-%EC%B0%A8%EC%9D%B4#uri_/_url_/_urn_%EA%B5%AC%EB%B6%84%ED%95%98%EA%B8%B0)

우리가 브라우저를 통해 웹을 이용하게 된다면, URL 단어는 익숙할 것이다. 하지만 가끔 뭔가 비스무리하면서도 다른 URI와 URN 이라는 단어를 사용하기도 하는데 이번 시간에는 URL / URI / URN 링크 문자의 명확한 차이를 알아보는 시간을 가져보자.

아래 그림에서 볼수 있듯이, URI는 URL과 URN을 포함하고 있다. 이들의 각 뜻은 다음과 같이 정의할 수 있다.

-   URI - 자원의 식별자
-   URL - 위치(Location)
-   URN - 이름(Name)

___

### **URI / URL / URN 정의**

#### **URI (Uniform Resource Identifier)****

-   통합 자원 식별자(Uniform Resource Identifier)는 인터넷에 있는 자원을 어디에 있는지 **자원 자체를 식별하는 방법**이다.
    -   Uniform : 리소스를 식별하는 통일된 방식
    -   Resource : 자원, URI로 식별할 수 있는 모든 것  
        여기서 자원은 웹 브라우저의 파일만 뜻하는 게 아니라, 실시간 교통정보 등 우리가 구분할 수 있는 것은 모든 게 리소스가 된다.
    -   Identifier : 다른 항목과 구분하는데 필요한 정보
-   URI의 존재는 인터넷에서 요구되는 기본조건으로서 인터넷 프로토콜에 항상 붙어 다닌다.
-   URI의 하위개념으로 URL, URN 이 있다

#### **URL (Uniform Resource Locator)**

-   파일식별자(Uniform Resource Locator)는 네트워크 상에서 **자원이 어디 있는지 위치**를 알려주기 위한 규약이다.
-   즉, 컴퓨터 네트워크와 검색 메커니즘에서의 위치를 지정하는, 웹 리소스에 대한 참조이다.
-   흔히 우리는 URL을 웹 사이트 주소로만 알고 있지만, URL은 웹 사이트 주소뿐만 아니라 컴퓨터 네트워크상의 자원을 모두 나타내는 표기법이다.
-   그리고 해당 주소에 접속하려면 URL에 맞는 프로토콜(http, sftp, smp ..등)을 알아야 하고, 그와 동일한 프로토콜로 접속해야 한다.

> [![postcard-title](https://scrap.kakaocdn.net/dn/t2OtY/hyR5miemvG/FtZbCIK6MGJNTMi4Uzdf6k/img.jpg?width=800&height=466&face=0_0_800_466,https://scrap.kakaocdn.net/dn/2tb8A/hyR5pF1r4K/0ikyzLWiPYZos7nwOGL8UK/img.jpg?width=800&height=466&face=0_0_800_466,https://scrap.kakaocdn.net/dn/fTaGs/hyR5qLHYBP/crEMUl1Mlyss711nyjwC71/img.png?width=1026&height=423&face=0_0_1026_423)](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-%EA%B5%AC%EC%84%B1-%EC%9A%94%EC%86%8C-%EC%9A%94%EC%B2%AD-%ED%9D%90%EB%A6%84-%EC%A0%95%EB%A6%AC)
> 
> URL 구성 이해하기 프로토콜 : https 호스트명 : <www.google.com> 포트번호 : 443 패스 : /search 쿼리 파라미터 : q=hello&hl=ko scheme 주로 프로토콜(어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙) 사용 예

#### **URN (Uniform Resource Name)**

-   통합 자원 이름(Uniform Resource Name)은 urn:scheme 을 사용하는 URI를 위한 역사적인 이름이다.
-   URL이 리소스가 있는 위치를 지정한다면, URN은 리소스에 이름을 부여하는 것이다.
-   URN은 영속적이고, 위치에 독립적인 자원을 위한 지시자로 사용하기 위해 1997년도 RFC 2141 문서에서 정의되었다.
-   하지만 리소스가 이름에 매핑되어 있어야 하기 때문에 이름으로 부여하면 거의 찾기가 힘들다. 그래서 대부분 URL만 쓴다. (즉 URN은 몰라도 된다)

___

### **URI / URL / URN 구분하기**

위에서 URI / URL / URN 의 정의에 대해 알아보았지만, 아직도 모호하다.

인터넷 상의 자원의 위치(URL)와 자원의 식별자(URI)는 언뜻 보면 같은 것을 의미하는 듯 하다. 하지만 '자원의 위치'라는 것은 결국은 '하나의 **파일 위치**'를 나타내는 것임을 명심하자.

예를들어 다음과 같은 홈페이지 링크가 있다고 하자.

<http://www.naver.com/index.html?page=1232950&id=776>

<http://www.naver.com/> 서버에 위치한 index.html 페이지는 query string인 page의 값에 따라 여러가지 화면 결과를 나타나게 된다.

이때 여기서 URL은 index.html의 위치를 표기한 <http://www.naver.com/index.html> 까지이다.

하지만 사용자가 원하는 정보에 도달 하기위해서는 ?page=1232950&id=776라는 식별자(Identifier)가 필요한 것이다.

따라서 엄격히 구분하자면 위의 <http://www.naver.com/index.html?page=1232950&id=776> 주소는 URI이고, 식별자가 빠진 <http://www.naver.com/index.html를> URL이라고 하는 것이다. 

이유는 **URL은 자원의 위치**를 나타내 주는 것이고 **URI는 자원의 식별자**인데, ?page=1232950&id=776 이 부분은 위치를 나타내는 것이 아니라 page값이 1232950이고 id가 776인 것을 나타내는 식별하는 부분이기 때문이다.

물론 통상적으로 대충 URL이라고 얘기를 하지만 엄격하게는 URI라고 하는 것이 맞다.

조금 억지스러운 예를 들면, 아래의 두 주소는 같은 URL이고 다른 URI라고 할 수 있다.

<http://www.naver.com/index.html?page\=1232950&id=776>

<http://www.naver.com/index.html?page\=9923145&id=122>

___

## 참고자료

<https://chobopark.tistory.com/221>

<https://stackoverflow.com/questions/4913343/what-is-the-difference-between-uri-url-and-urn>

인용한 부분에 있어 만일 누락된 출처가 있다면 반드시 알려주시면 감사하겠습니다

이 글이 좋으셨다면 **구독 & 좋아요**

여러분의 구독과 좋아요는 저자에게 큰 힘이 됩니다.

![subscribe](https://tistory1.daumcdn.net/tistory/4939852/skin/images/sub.png)
