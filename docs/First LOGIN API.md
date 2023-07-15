---
description:
aliases: 
tags: 
created: 2023-04-20T22:21:09
updated: 2023-07-15T21:33:04
title: First LOGIN API
---

- 커스텀 에러타입을 만들어 로그인 실패에 대응하기
	- `Debug`, `std::fmt::Display` 트레잇을 구현하여 무슨 에러인지 확인 가능하게
	- [`IntoResponse`](https://docs.rs/axum/latest/axum/response/trait.IntoResponse.html) axum 트레잇을 구현하여 핸들러가 해당 타입을 리턴할 수 있게 만들어주자 [[axum Response]]
	- 에러타입을 `Response`타입으로 만들어주는 이유는 바로 에러 자체를 클라이언트에게 직송하면 안되기 때문이다.
- `web` 모듈을 만들어 모든 유용한 라우터들을 여기에서 관리하도록 만들어보자.
	- `/src/web/mod.rs` 파일을 만들게 되면 자동으로 `web` 모듈이 생성이 된다. `main.rs`에서 나는 `mod web`을 하게 되면 해당 모듈의 내용을 임포트 하게 된다.
	- `web` 모듈에서 위에와 똑같은 방식으로 서브모듈인 `routes_login` 모듈을 만들 것이다. `web::routes_login` 모듈이라니... 어딘가 굉장히 conventional하다. 이런 구조 아주 칭찬해!! 
	- 로그인 핸들러를 구현하고 (`api_login`) 해당 핸들러를 라우팅하는 라우터(`routes`)까지 구현했다. 이때 [[serde_json crate]]를 사용하여 Json 객체를 아주 쉽게 불러와서 또 우리가 만든 `Error`타입을 에러로 가지는 `Result` 타입으로 리턴할 수 있게 만들었다. 로그인에 성공했을 때 반환하는 타입으로 `Json`을 지정했는데, `json!` 매크로로 아주 쉽게 만들 수 있었다.
- 이제 `quick_dev.rs` 파일로 가서 우리가 싸지른 코드가 얼마나 잘 동작하는지 확인해야한다. login api는 `POST`로 구현했기 때문에 `httpc_test` 크레이트의 `do_post` 함수를 사용했다. 
	- 아래는 성공적으로 로그인이 되었을 때의 모습이다.
	- ![[Pasted image 20230420232311.png]]
	- 아래는 일부로 틀린 비밀번호를 입력했을 때 나온 모습이다. 우리가 만든 커스텀 에러타입 구현시 작성해놓은 `axum::http::StatusCode::INTERNAL_SERVER_ERROR`의 에러넘버가 500이라는 것을 알 수 있었고, `Response Body` 자리에 튜플의 두번째 자리에 적은 문자열이 고대로 들어가 있는 것을 확인할 수 있었다.
	- ![[Pasted image 20230420232853.png]]
- [middleware??](https://youtu.be/XZtlD_m59sM?t=1247)이게뭐냐? `routes_all`에다가  `middleware::map_response` 를 했더니 레이어를 하나 더 추가해버렸넹?? 그래서 내가 라우팅하는 모든 Response들을 전부 커스텀 response mapper에 통과시켜버린 뒤에야 작업을 수행하게 만들어줬다. 신기허네...
