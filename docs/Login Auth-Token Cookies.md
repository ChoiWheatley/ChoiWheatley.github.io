---
description:
aliases: 
tags: 
created: 2023-04-23T20:41:10
updated: 2023-07-15T21:33:04
title: Login Auth-Token Cookies
---

- 쿠키는 `tokio` 프레임워크의 코어인 `tower`의 크레이트 `tower-cookies`를 사용한다.
- 쿠키는 key와 value 쌍으로 이루어져 있으며, value는 다시 username, userid, expression date, signature 순으로 이루어져 있다. 
- 우리가 지난 시간에 만들었던 핸들러 `api_login` 의 파라메터로 쿠키를 추가한다. ~~아니 함수 오버로딩이 없는데..?~~ 이 핸들러 내부는 클라이언트(웹 브라우저)의 요청에 응답하는 서버의 내용이므로, [`Cookies::add`](https://docs.rs/tower-cookies/latest/tower_cookies/struct.Cookies.html#method.add) 메서드를 사용하여 클라이언트에게 "쿠키를 추가하라"는 명령을 전달할 수 있다.
- [[Login Auth-Token Cookies#^j5gkbi|최초 api login]]시 쿠키를 `set`하는 명령이 서버에서 전송이 됐고, 이를 [`Response cookie`](https://stackoverflow.com/questions/34215022/whats-the-difference-between-request-cookie-and-reponse-cookie#:~:text=Response%20cookies%20are%20the%20cookies%20created%20on%20server,cookies%20contained%20in%20the%20value%20of%20this%20header.) 에 담겨서 클라이언트(웹 브라우저)에게 전달이 된다. 클라이언트는 해당 값을 고대로  `Client cookie`에 저장하게 된다.
- 한 번 클라이언트에 저장이 된 쿠키는 다른 API를 호출할 때라도 언제나 들고 있게 된다. 예를 들어 `hello2` API를 다시 한 번 호출했을 때에도 [[Login Auth-Token Cookies#^rwj2f7|다음과 같이]] `Client cookies` 칼럼에 값을 가지고 있게된다.

---

![[Pasted image 20230423204300.png]] ^j5gkbi  
![[Pasted image 20230423204824.png]] ^rwj2f7
