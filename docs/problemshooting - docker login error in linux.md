---
aliases: 
tags: 
description:
title: problemshooting - docker login error in linux
created: 2024-10-18T22:12:53
updated: 2024-10-18T22:41:08
---
parent:
- [[docker 교과서 Chapter 5]]  
references:
- [docker/login/#credential-stores](https://docs.docker.com/reference/cli/docker/login/#credential-stores)
- <https://www.passwordstore.org>
- [pass init 관련 옵션](https://git.zx2c4.com/password-store/about/#COMMANDS)

---

## 시스템 정보

- OS: Archlinux

## 발단

처음에 도커 로그인을 진행했을때 Warning이 떴기 때문에 도커 문서를 확인하며 `pass` CLI 도구를 설치 및 초기화 하여 비밀번호 관리자를 정의했다.

```
$ docker login --username $DOCKERID
Password:
WARNING! Your password will be stored unencrypted in /home/choiwheatley/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credential-stores

Login Succeeded
```

그리고 다시 로그인을 수행했을때 GPG 쪽에서 문제가 발생한 듯 했다.

```
$ pass init "chltmdgus604@gmail.com"
$ docker login --username $DOCKERID
Password:
Error saving credentials: error storing credentials - err: exit status 1, out: `exit status 1: gpg: err
or retrieving 'chltmdgus604@gmail.com' via WKD: No data
gpg: chltmdgus604@gmail.com: skipped: No data
gpg: [stdin]: encryption failed: No data
Password encryption aborted.`
```

여기에서 가장 눈여겨 보아야 하는 것은 다음 문장이다. 여기서 WKD는 Web Key Directory의 약자로, GPG가 적절한 키를 찾지 못했다는 것을 의미한다. 맞는 말이다. 나는 gpg를 사용하여 키를 생성한 적이 없었다.

> gpg: error retrieving ~ via WKD: No data

## 해결방법

`pass`를 처음 설치하고 init을 해줘야 한다. ← gpg-id가 필요하다.

따라서 `gpg --generate-key` 옵션을 사용하여 키를 생성해야 한다. 아래와 같은 대화형 인터페이스를 사용하여 이름과 이메일주소, 비밀번호를 모두 작성하면...

```shell
$ gpg --generate-key
gpg (GnuPG) 2.4.5; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Note: Use "gpg --full-generate-key" for a full featured key generation dialog.

GnuPG needs to construct a user ID to identify your key.

Real name: ChoiWheatley
Email address: chltmdgus604@naver.com
You selected this USER-ID:
    "ChoiWheatley <chltmdgus604@naver.com>"

Change (N)ame, (E)mail, or (O)kay/(Q)uit?
```

![[Screenshot 2024-10-18 at 22.32.56.png]]

gpg-id를 발급받은 것을 확인할 수 있다. `gpg --list-key` 옵션을 사용하여 `pub`에 해당하는 문자열이 키이다.

```shell
$ gpg --list-key
[keyboxd]
---------
pub   ed25519 2024-10-18 [SC] [expires: 2027-10-18]
      CXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
uid           [ultimate] Choi Seunghyeon <chltmdgus604@gmail.com>
sub   cv25519 2024-10-18 [E] [expires: 2027-10-18]

```

이 키를 복사하여 `pass init <gpg-id>`를 채워주면 된다.

## 참고

`docker login`을 마친 후 최초로 `docker push`를 수행하기 위해선 호스트 컴퓨터에서 GPG 비밀번호를 입력해야 한다. 나는 SSH 접속한 상태에서 계속 push 했더니 denied가 떴었다.

```
$ docker push $DOCKERID/image-gallery:v1
The push refers to repository [docker.io/chltmdgus604/image-gallery]
7ad9b9de3d45: Preparing
e360d44a9d56: Preparing
e94a8ffca97d: Preparing
f87269b94a71: Preparing
89ae5c4ee501: Preparing
denied: requested access to the resource is denied
```
