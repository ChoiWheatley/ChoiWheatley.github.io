---
aliases: 
tags: 
description:
created: 2023-06-21T20:39:04
updated: 2024-07-18T16:33:48
title: oauth {django}
---
- [YT](https://www.youtube.com/watch?v=Gk9tsLHMMsM&list=WL&index=1&t=812s)
- [notion / 주백개그이 / 인증 인가](https://www.notion.so/estsoft-junior-backend/9-af8696470fdd433c82d3d59c4b212641?pvs=4#7c4c8eeca4be49ebb88409e4507b0b6a)
- allauth 의존성 추가
- api 같은 건 그냥 발급받는게 아니었다. 구글 내에서 세팅해야할 게 좀 많았다.
- `AUTHENTICATION_BACKENDS`라는 걸 설정하네? 

- [allauth {python module}](https://allauth.org)
	- <https://console.cloud.google.com/>
	- <https://developers.naver.com/main/>
	- <https://developers.kakao.com/console>

[[소셜 로그인 마지막 단계에서의 리디렉션]]

## 9.2 **OAuth 2.0 기본 개념**

**4가지 역할**

**2-1-1. 리소스 소유자 Resource Owner**

리소스 소유자는 사용자로서 자신의 데이터 또는 보호된 리소스에 대한 접근 권한을 가지고 있는 주체입니다. 일반적으로 웹 서비스나 애플리케이션에 로그인한 사용자를 의미합니다. 리소스 소유자는 클라이언트에 대한 접근 권한을 부여하고 액세스를 제어할 수 있습니다.

**2-1-2. 리소스 서버 Resource Server 또는 보호된 리소스 Protected Resource**

리소스 서버는 리소스 소유자가 보호하고 있는 실제 리소스를 호스팅하고 관리하는 서버입니다. 이 서버는 클라이언트가 액세스 권한을 얻었을 때 해당 리소스에 대한 요청을 처리하고 응답합니다. 예를 들어, 소셜 미디어 플랫폼의 사진이나 동영상을 저장하고 제공하는 서버가 리소스 서버가 될 수 있습니다.

**2-1-3. 클라이언트 Client**

클라이언트는 리소스 서버에 접근하기 위해 권한을 요청하는 애플리케이션이나 서비스를 의미합니다. 일반적으로 웹 애플리케이션, 모바일 앱, 데스크톱 애플리케이션 등이 클라이언트 역할을 수행할 수 있습니다. 클라이언트는 리소스 소유자를 대신하여 리소스 서버에 접근 권한을 요청하고, 액세스 토큰을 받아 실제 리소스에 액세스할 수 있습니다.

**2-1-4. 인가 서버 Authorization Server**

인가 서버는 클라이언트의 인증 및 인가를 처리하는 서버입니다. 클라이언트가 리소스 서버에 접근하기 위한 액세스 토큰을 얻기 위해 인가 서버에게 인증 정보를 제공하고 권한 부여를 요청합니다. 인가 서버는 리소스 소유자를 인증하고, 클라이언트에게 액세스 토큰을 발급하며, 해당 토큰을 사용하여 리소스 서버에 대한 인가된 액세스를 제공합니다.

**2가지 토큰**

**2-2-1. 액세스 토큰 Access Token**

액세스 토큰은 클라이언트에게 권한이 위임됐다는 것을 나타내기 위해 인가 서버가 클라이언트에게 발급합니다. OAuth에서는 토큰의 포맷이나 내용을 따로 정의하지 않습니다. 그래서 형식이 명확하지 않기때문에 클라이언트는 토큰을 분석할 필요가 없습니다.

인가 서버는 토큰을 만들어 발급하고 리소스 서버는 이 토큰을 검증하여 보호된 리소스에 접근을 허용하기 때문에 토큰을 이해하고 어떤 내용을 의미하는지 알 수 있습니다. 이러한 방식을 채택함으로써 클라이언트는 좀 더 단순해지고 서버 측은 토큰을 배포하는 방법에 있어 엄청난 유연성을 얻을 수 있게됩니다.

**2-2-2. 리프레시 토큰 Refresh Token**

사용자가 토큰을 폐기 하거나, 토큰이 만료되거나, 다른 이유로 토큰이 유효하지 않게 될 수 있습니다. 그렇게 되면 리소스 서버에 요청을 했을 때 에러를 반환하게 됩니다. 그 때 리프레시 토큰을 사용하여 인가 서버로부터 새로운 액세스 토큰을 발급 받을 수 있습니다.

OAuth 1.0에서는 리프레시 토큰이 존재하지 않았기 때문에 액세스 토큰이 유효하지 않게 된 경우 리소스 소유자에게 의지할 수밖에 없었습니다. 그런 상황을 피하기 위해 OAuth 1.0에서 선택한 방법은 토큰이 폐기될 때까지 영원히 사용하는 것이었습니다. 이것은 토큰을 탈취한 공격자 역시 해당 토큰을 영원히 사용할 수 있다는 의미이기도 합니다.

이러한 취약점을 보완하기 위해 OAuth 2.0에서는 토큰의 유효 기간이 자동으로 설정되도록 했습니다. 또한 토큰의 유출을 개별적으로 제한하기 위해서 리프레시 토큰은 리소스 서버에 전달되지 않고 오로지 새로운 액세스 토큰을 요청하는 용도로만 사용됩니다.

**4가지 승인 종류**

**2-3-1. 인가 코드 승인 Authorization Code Grant**

![https://www.ibm.com/docs/en/tfim/6.2.2.6?topic=overview-oauth-20-workflow](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d8270c3e-3c65-4a86-970e-20af380fa547/image4.png)

[https://www.ibm.com/docs/en/tfim/6.2.2.6?topic=overview-oauth-20-workflow](https://www.ibm.com/docs/en/tfim/6.2.2.6?topic=overview-oauth-20-workflow)

1. 클라이언트 자격 증명을 사용하여 인가 서버에 액세스 토큰을 요청합니다.
2. 인가 서버는 클라이언트 자격 증명의 유효성을 검사하고 유효하다면 액세스 토큰과 리프레시 토큰을 발급합니다.
3. 클라이언트는 액세스 토큰을 사용하여 리소스 서버에 보호된 리소스를 요청합니다.
4. 리소스 서버는 토큰의 유효성을 검사하고 유효하다면 요청에 따른 응답을 제공합니다.
5. 액세스 토큰이 만료될 때까지 3단계 4단계를 반복합니다. 클라이언트 측에서 액세스 토큰이 만료된 것을 알고있다면 7단계로 건너뜁니다.
6. 액세스 토큰이 유효하지 않으면 리소스 서버가 오류를 반환합니다.
7. 리프레시 토큰을 사용하여 새 액세스 토큰을 요청합니다.
8. 인가 서버는 클라이언트 자격 증명과 리프레시 토큰의 유효성을 검사하고 유효하다면 새 액세스 토큰과 리프레시 토큰을 발급합니다.

**2-3-2. 암묵적 승인 Implicit Grant**

![https://www.ibm.com/docs/en/tfim/6.2.2.6?topic=overview-oauth-20-workflow](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9c736fbc-c1be-42d1-a811-40e6cd3fc176/image5.png)

[https://www.ibm.com/docs/en/tfim/6.2.2.6?topic=overview-oauth-20-workflow](https://www.ibm.com/docs/en/tfim/6.2.2.6?topic=overview-oauth-20-workflow)

1. 클라이언트는 리소스 소유자의 유저 에이전트를 인가 엔드포인트로 안내하여 흐름을 시작합니다. 여기에서 유저 에이전트란 리소스 소유자가 사용 중인 운영체제와 브라우저의 버전, 종류 등을 담은 정보입니다. 클라이언트는 요청 범위, 클라이언트 식별자, 로컬 상태 및 리다이렉트 URI를 포함하여 인가 서버로 요청을 보냅니다. 인가 서버는 접근을 허용하거나 거부한 후 유저 에이전트를 다시 리다이렉트 URI로 보냅니다.
2. 인가 서버는 유저 에이전트를 통해 리소스 소유자를 인증하고 리소스 소유자의 접근을 허용할지 또는 거부할지 여부를 설정합니다
3. 리소스 소유자가 접근을 허용하는 경우 인가 서버는 앞서 제공된 리다이렉트 URI를 사용하여 유저 에이전트를 클라이언트로 리다이렉트 시킵니다.이 때 리다이렉트 URI에는 URI 프래그먼트에 담긴 액세스 토큰이 포함됩니다.
4. 유저 에이전트는 프래그먼트 정보를 로컬로 유지합니다.
5. 웹 서버는 일반적으로 스크립트가 포함된 HTML 웹 페이지를 반환합니다. 웹 페이지는 유저 에이전트가 보유한 프래그먼트를 포함한 전체 리다이렉트 URI에 접근합니다. 또한 프래그먼트에 포함된 액세스 토큰 및 기타 파라미터를 추출할 수도 있습니다.
6. 유저 에이전트는 웹 서버에서 제공하는 스크립트는 로컬로 실행하여 액세스 토큰을 추출하고 이를 클라이언트에 전달합니다.

**2-3-3. 리소스 소유자 암호 자격 승인 Resource Owner Password Credentials Grant**

![https://www.ibm.com/docs/en/tfim/6.2.2.6?topic=overview-oauth-20-workflow](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c54aa690-74c2-4394-ae72-16096463db50/image15.png)

[https://www.ibm.com/docs/en/tfim/6.2.2.6?topic=overview-oauth-20-workflow](https://www.ibm.com/docs/en/tfim/6.2.2.6?topic=overview-oauth-20-workflow)

1. 리소스 소유자는 클라이언트에게 아이디와 비밀번호를 제공합니다.
2. 클라이언트는 토큰 엔드포인트를 통해 인가 서버에 액세스 토큰을 요청합니다. 여기에는 리소스 소유자로부터 받은 자격 증명을 포함합니다.
3. 인가 서버가 리소스 소유자와 클라이언트의 자격 증명의 유효성을 검사한 후에 액세스 토큰을 발급하고 선택적으로 리프레시 토큰을 발급합니다.

**2-3-4. 클라이언트 자격 승인 Client Credentials Grant**

클라이언트 자격 승인 흐름은 클라이언트 자격 증명만 사용하여 액세스 토큰을 요청할 때 사용됩니다. 다음 상황 중 하나에 적용할 수 있습니다.

1. 클라이언트가 자신이 제어하는 리소스에 액세스를 요청하는 경우
2. 클라이언트가 인가 서버에서 권한 부여가 정리된 다른 보호된 리소스를 요청하는 경우

![https://www.ibm.com/docs/en/tfim/6.2.2.6?topic=overview-oauth-20-workflow](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/290e3d74-9517-462a-83d6-2e6842614872/image29.png)

[https://www.ibm.com/docs/en/tfim/6.2.2.6?topic=overview-oauth-20-workflow](https://www.ibm.com/docs/en/tfim/6.2.2.6?topic=overview-oauth-20-workflow)

1. 클라이언트 자격 증명으로 인증하여 토큰 엔드포인트에서 액세스 토큰을 요청합니다.
2. 인가 서버가 자격 증명의 유효성을 검사한 후 액세스 토큰을 발급합니다.
