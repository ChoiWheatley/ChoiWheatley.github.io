---
description:
aliases: 
tags: 
created: 2023-04-18T22:20:41
updated: 2023-07-11T15:21:07
title: query parameters with question mark and Query types
---
- [axum::extract::Query](https://docs.rs/axum/latest/axum/extract/struct.Query.html)
- [Query Parameters](https://im-designloper.tistory.com/19)
- URL 주소에 쿼리문을 넣을 수 있다. 
- https://youtu.be/XZtlD_m59sM?t=506
- 위의 영상과 같이 주소 뒤 라우트 뒤에 `?attribute=value` 문법으로 작성하면 알아서 메서드를 호출할 때 쿼리문도 같이 날린다. 아래 스크린샷은 예정된 쿼리문을 받아 Html 튜플구조체에 작성하는 코드를 나타낸다.
- ![[Pasted image 20230418222155.png]]
```rust
#[tokio::main]
async fn main() {
    let routes_hello = Router::new().route(
        "/hello", // path
        axum::routing::get(handler_hello),
    );
    let addr = SocketAddr::from(([127, 0, 0, 1], 8080));
    println!("@@@@@ LISTENING ON {}", addr);
    axum::Server::bind(&addr)
        .serve(routes_hello.into_make_service())
        .await
        .unwrap();
}

async fn handler_hello(Query(params): Query<HelloParams>) -> impl IntoResponse {
    println!("@@@@@ {:<12} - {params:?}", "HANDLER");
    let name = params.name.unwrap_or("World".to_string());
    Html(format!("Hello <b>{name}!!!</b>"))
}
```