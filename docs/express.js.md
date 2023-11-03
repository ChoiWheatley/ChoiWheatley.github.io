---
aliases: 
tags: 
description:
title: express.js
created: 2023-11-01T16:02:37
updated: 2023-11-03T20:13:56
---
- [[0018 Javascript ☕️]]
- [Node.js 교과서 개정 3판 - 웹 아카이브 링크](https://thebook.io/080334/)
- <https://expressjs.com/>
___

## README

node.js 환경의 웹 개발 프레임워크중 하나인 express를 공부하면서 배운 내용을 아카이브 합니다. 빠른 지식습득을 위해 과제를 먼저 진행하고 부족한 부분은 그때그때 찾아가는 식으로 진행할 예정입니다.

**목표**

- HTTP 요청에 대하여 적절한 응답을 보낼 수 있다.
- 데이터베이스(처음엔 MongoDB, 나중엔 MySQL)에 ORM을 사용하여 쿼리문을 날릴 수 있다.
- 요구사항에 따라 게시판 사이트를 위한 백엔드 서버를 만들 수 있다.

## Structure of express.js

[[structure of express.js]]

**ai 요약**

- `app.js` 파일에는 메인 루틴이 포함되어 있으며, 다른 모듈을 가져와 URL을 라우팅할 수 있습니다.
- `package-lock.json` 파일은 설치된 패키지의 메타데이터를 저장하며, 이 파일을 통해 다른 개발자도 동일한 패키지 설정을 공유할 수 있습니다.
- `package.json` 파일은 패키지의 메타데이터를 정의합니다.
- `node_modules` 폴더는 npm을 통해 설치된 패키지 의존성이 포함되어 있습니다.
- `routes` 폴더는 사용자가 만든 디렉토리로, 하위 모듈들을 포함합니다.

`app.js` 파일은 express 객체와 다른 모듈을 가져와 express 앱을 실행하고 듣습니다. 또한 라우팅을 처리하고 다른 모듈을 등록합니다. 이때, `require` 문법이 사용되는 이유는 CommonJS 스펙에 따라 Node.js 환경에서 모듈을 가져오기 위해서입니다.

라우팅은 `express` 모듈을 통해 기본 라우팅을 설정하며, 타 모듈을 `app`에 등록시키기 위해 미들웨어를 사용합니다.

`routes/goods.js` 모듈은 라우터 미들웨어를 사용하기 위해 `express.Router()` 객체를 가져오고, 해당 라우터를 export하여 다른 모듈에서 사용할 수 있도록 합니다.

## mongodb

[[mongoose, a mongodb ODM for javascript]]

**ai 요약**

웬만한 경우에, MongoDB를 WSL2에서 실행할 때 포트가 열려있지 않은 문제가 발생할 수 있습니다. 이 경우 Docker를 사용하여 간단하게 문제를 해결할 수 있습니다. 더 자세한 내용은 [이 링크](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-community-with-docker/)를 참조하십시오.

또한, API를 통해 새로운 상품을 추가하고 장바구니에 상품을 추가하거나 수량을 변경하는 방법에 대한 내용도 포함되어 있습니다. 또한 보안을 강화하기 위해 MongoDB에 인증 절차를 추가해야 합니다.

`return res.json(...)`와 `res.json(...)`의 사용은 요청이 유효하지 않은 경우와 정상적인 경우에 각각 사용됩니다. 전자는 오류가 발생했음을 나타내고, 후자는 요청이 성공적으로 처리되었음을 나타냅니다.

## MySQL

[[sequlize, a MySQL ORM for javascript]]

## JWT

[[jsonwebtoken npm]]

**ai 요약**

**jsonwebtoken 패키지**는 JSON 웹 토큰(JWT)을 생성하고 검증하는 데 사용되는 Node.js의 라이브러리입니다. 이 라이브러리를 사용하면 암호화된 정보를 포함하는 JWT를 생성하고, 해당 토큰을 복호화하여 원래 정보를 추출할 수 있습니다. 

JWT는 누구나 복호화할 수 있기 때문에 비밀번호와 같은 민감한 정보를 저장해서는 안 됩니다. JWT의 주요 목적은 페이로드의 변조 여부를 확인하기 위한 것이므로 주의가 필요합니다.

**Express.js**는 Node.js 웹 애플리케이션 프레임워크로, 웹 서버 및 API를 쉽게 작성하고 구축할 수 있도록 도와줍니다. Express.js는 미들웨어를 활용하여 다양한 기능을 구현할 수 있으며, 강력한 라우팅 기능을 제공합니다.

**HTTPS와 JWT, 쿠키, 세션에 관한 보안**은 웹 보안에 관한 중요한 주제입니다. HTTPS는 데이터를 암호화하여 보호하고, JWT는 토큰 기반의 인증을 제공합니다. 쿠키와 세션은 사용자 인증과 관련된 데이터를 유지하고 관리하는 데 사용됩니다. 이러한 보안 기술들은 개발자가 안전한 웹 애플리케이션을 구축하는 데 중요한 역할을 합니다. 

로그인 API 예제에서는 사용자 정보를 JWT로 생성하고, 해당 토큰을 쿠키에 할당하여 사용자에게 반환하는 과정을 보여줍니다. Bearer 토큰 형식으로 JWT를 전달하여 인증을 수행합니다. 

## DUMP

- path에 인자를 집어넣는 방식이 장고와는 다르게 타입을 따로 집어넣을 수 없나? 예를 들어 "/goods/carts"와 "/goods/2"를 구분하고 싶은데 
	- [`router.param(name, callback)`](https://expressjs.com/en/4x/api.html#router) 을 사용하면 `router.<method>(path, callback)`에 정의된 path의 파라메터를 처리하는 로직을 정의할 수 있다고 한다. 내가 별도로 호출할 필요도 없다는게 신기하네.
	- 그래서 저 `next` 함수의 역할은 뭔데?
- 노션으로 작성된 API 명세서 예제
	- ![[Pasted image 20231103201414.png]]
