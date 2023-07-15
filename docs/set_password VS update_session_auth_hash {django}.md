---
description:
aliases: 
tags: 
created: 2023-06-08T12:30:27
updated: 2023-07-15T21:33:03
title: set_password VS update_session_auth_hash {django}
---
- [github issue](https://github.com/ESTsoft-Book-Project/bookstore/issues/15)
- set_password의 경우 사용자의 비밀번호만 변경하는 함수입니다. 그래서 해당 함수만 사용하여 비밀번호 변경을 시도할 경우, 기 인증된 사용자의 상태와 달라지기 때문에 로그아웃 하게 됩니다. update_session_auth_hash 함수를 함께 사용할 경우 기 인증되었던 세션에서 비밀번호가 변경된 세션으로 갱신해주기 때문에 로그인 상태를 유지할 수 있게 됩니다.
