---
aliases: 
tags: 
description:
created: 2023-06-24T08:33:57
updated: 2023-11-07T06:17:44
title: Securities about {https} and {jwt {cookie}, {session}}
---

## HTTPS

SSL/TLS(Secure Sockets Layer/Transfer Layer Security)라고 불리우는 암호체계를 사용하여 민감한 정보를 암호화/복호화 하는 기술입니다. 비대칭키를 사용하기 때문에 Public Key를 사용해 암호화한 본문은 Private Key를 가지고 있는 사람만이 복호화 할 수 있습니다.

wireshark, nmap과 같은 소프트웨어를 사용하면 패킷을 통해 헤더, 본문을 읽어볼 수 있게됩니다. 심지어 통신을 가로채 본문에 악성코드나 광고를 삽입할 수도 있게 되는데, 평문이 오갈 경우, 이를 막을 방법이 없게 됩니다. 

따라서 많은 웹사이트는 HTTPS를 사용하여 자신의 웹 서비스를 호스팅하고 있습니다. HTTPS는 웹 서버가 신뢰할 수 있는 공인된 기관이 발급하는 디지털 증명서인 SSL/TLS Certificate를 사용해야 합니다. 웹 서버의 도메인, 회사 정보, 공개 키 등을 공인 기관에 제출하고 검증하는 절차를 거쳐야 합니다.

## JSON Web Token

- <https://en.wikipedia.org/wiki/JSON_Web_Token>
- <https://jwt.io/introduction>

JWT는 인증/인가(authentication/authorization)의 여러 방법 중 하나입니다. 대표적인 인증 방법으로는 쿠키(세션), 토큰, Third-party(OAuth, API-token), SAML, OpenID 등이  있습니다. JWT는 이 중에서 토큰 기반 기법을 사용합니다.

JWT는 세 가지의 구조로 이루어져 있습니다. Header, Payload, Signature.

- 헤더에는 알고리즘과 타입으로 이루어져 있습니다. 알고리즘으로는 리소스 서버가 해당 토큰을 만들 때 사용한 알고리즘을 정의하며, HMAC, SHA256, RSA 등을 명시할 수 있습니다. 타입은 "JWT" 고정입니다.

```json
{
	"alg": "SHA256",
	"typ": "JWT"
}
```

- 페이로드에는 claim이 담겨져 있는 공간입니다. claim이란, 데이터가 의미있는 정보를 담기 위해 필요한 사용자 정보에 대한 선언을 의미합니다. 주로 유저/관리자 식별, 만료일자, 이름 등에 대한 정보가 담겨 있습니다.

```json
{
	"iss": true,
	"admin": true,
	"name": "Choi Wheatley"
}
```

- 시그니처에는 헤더와 페이로드가 유효한지를 검증하기 위해 리소스 서버에서 토큰을 생성할 시에 계산되는 암호문입니다. Base64Url로 인코딩한 header와 payload를 `.`으로 구분한 뒤 리소스 서버가 가지고 있는 비밀 키 (secrete key)로 암호화를 진행합니다. 그 결과값이 곧 시그니처 입니다.

마지막으로 header, payload, signature를  `.`으로 구분한 Base64Url로 인코딩하여 붙인 긴 문자열을 클라이언트에게 반환합니다.

아래는 JWT 토큰을 생성하는 수도코드 입니다.

```python
header = {"alg": "HS256", "typ": "JWT"}
payload = {"iss": true, "admin": true, "name": "Choi Wheatley"}
signature = HMACSHA256(
	base64UrlEncode(header) + "." +
	base64UrlEncode(payload)
)
token = base64UrlEncode(header) + '.' +
	base64UrlEncode(payload) + '.' +
	base64UrlEncode(signature)
```

---
이 아래는 집필 프로젝트 외적으로 다루는 공간임
- [얄코 HTTPS와 대칭키, 비대칭키](https://youtu.be/H6lpFRpyl14)
- [얄코 JWT](https://youtu.be/1QiOXWEbqYQ)
- [포프TV JWT](https://youtu.be/MUUqogMpGiA)
- [코딩애플 JWT 대충 쓰면 코딩인생 끝남](https://youtu.be/XXseiON9CV0)
- [medium.com](https://medium.com/@vivekmadurai/different-ways-to-authenticate-a-web-application-e8f3875c254a)

## access token & refresh token

**2-2-1. 액세스 토큰 Access Token**

액세스 토큰은 클라이언트에게 권한이 위임됐다는 것을 나타내기 위해 인가 서버가 클라이언트에게 발급합니다. OAuth에서는 토큰의 포맷이나 내용을 따로 정의하지 않습니다. 그래서 형식이 명확하지 않기때문에 클라이언트는 토큰을 분석할 필요가 없습니다.

인가 서버는 토큰을 만들어 발급하고 리소스 서버는 이 토큰을 검증하여 보호된 리소스에 접근을 허용하기 때문에 토큰을 이해하고 어떤 내용을 의미하는지 알 수 있습니다. 이러한 방식을 채택함으로써 클라이언트는 좀 더 단순해지고 서버 측은 토큰을 배포하는 방법에 있어 엄청난 유연성을 얻을 수 있게됩니다.

**2-2-2. 리프레시 토큰 Refresh Token**

사용자가 토큰을 폐기 하거나, 토큰이 만료되거나, 다른 이유로 토큰이 유효하지 않게 될 수 있습니다. 그렇게 되면 리소스 서버에 요청을 했을 때 에러를 반환하게 됩니다. 그 때 리프레시 토큰을 사용하여 인가 서버로부터 새로운 액세스 토큰을 발급 받을 수 있습니다.

OAuth 1.0에서는 리프레시 토큰이 존재하지 않았기 때문에 액세스 토큰이 유효하지 않게 된 경우 리소스 소유자에게 의지할 수밖에 없었습니다. 그런 상황을 피하기 위해 OAuth 1.0에서 선택한 방법은 토큰이 폐기될 때까지 영원히 사용하는 것이었습니다. 이것은 토큰을 탈취한 공격자 역시 해당 토큰을 영원히 사용할 수 있다는 의미이기도 합니다.

이러한 취약점을 보완하기 위해 OAuth 2.0에서는 토큰의 유효 기간이 자동으로 설정되도록 했습니다. 또한 토큰의 유출을 개별적으로 제한하기 위해서 리프레시 토큰은 리소스 서버에 전달되지 않고 오로지 새로운 액세스 토큰을 요청하는 용도로만 사용됩니다.
