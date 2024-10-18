---
aliases: 
tags: 
description:
title: docker 교과서 Chapter 5
created: 2024-10-18T20:16:01
updated: 2024-10-18T23:39:42
---

## 도커 이미지 명명법

도커 레지스트리는 도커 이미지를 저장하는 보관소로, 별도로 지정하지 않으면 도커허브 (`github.io`)가 기본 설정된다. 따라서 우리가 줄기차게 pull 받았던 `diamol/base` 이미지를 예로 들면 아래와 같은 명칭으로 치환된다:

```
docker.io/diamol/base:latest
^         ^      ^    ^
|         |      |    └── tag
|         |      └─────── image
|         └────────────── group
└──────────────────────── registry domain
```

이미지 이름과 태그를 지정하는 방법은 아래와 같다:

```
$ docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

도커 이미지를 확인해보면 같은 ID를 가진 두개의 서로 다른 이름의 이미지를 볼 수 있을 것이다.

## 도커 레지스트리에 내 이미지를 푸시해보자

도커허브 (docker.io)가 기본설정으로, 웹 브라우저로 접속할땐 hub.docker.com 으로 들어가야 한다. 회원가입을 하고 터미널에서 `docker login --user <user-id>` 를 사용하여 로그인을 수행하면 된다.

[[problemshooting - docker login error in linux]]

로그인이 정상적으로 완료되었다면, `docker image push` 명령을 사용하면 된다.

```
$ docker image push [OPTIONS] NAME[:TAG]
```

## 도커 레지스트리도 도커 이미지로 제공이 된다

<https://distribution.github.io/distribution/about/deploying/> 를 참고하면 도커 이미지 레지스트리또한 서버이므로, 도커 허브와 동일하게 레이어 캐시 시스템을 사용하여 이미지를 내려받고 푸시하는 기능을 제공할 수 있다.

로컬에서 도커 이미지를 제공하는 방법은 도커 허브나 Amazon Elastic Container Registry (ECR), Google Container Registry (GCR) 등과 같은 클라우드 서비스를 이용하는 것과는 다르게 인터넷 다운에 대응하는 백업으로써 활용이 가능하다.

## 태그 명명규칙

도커 태그는 <https://semver.org> 의 규칙을 따른다. 뭐냐고? 

```
Major.Minor.Patch
```

두 개의 `.`으로 구분된 세개의 숫자로 정의하며, 자세한 내용은 해당 사이트 참고 바란다. 도커는 pull 받을때의 태그 버전의 정확도에 따라서 최신화 정도를 나눌 수 있다. 예를 들어 `hello:1`은 Major 버전 1버전의 가장 최신 Minor, Patch를 가져오고, `hello:1.2`는 Major 1버전, Minor 2버전의 가장 최신 Patch를 가져온다. 언제나 가장 최신 이미지를 가져오고 싶다면 `hello:latest`를 사용하면 된다.

여러 기기에서 동일한 작동을 보장하기 위해서는 가능한 버전 정확도를 높이는 것이 중요하며, 반대로 보안패치나 기능추가에 민감한 경우라면 버전 정확도를 낮추는 것이 중요하다.

## 골든 이미지와 기반 이미지

**기반 이미지**는 검증된 퍼블리셔 (마소, 구글 등)들이 만들어낸 SDK나 런타임 등을 의미한다. 많은 경우 이 기반 이미지로부터 빌드를 수행하거나 이미지를 실행시키는데 사용할 것이다. 그렇지만 얼마 지나지 않아 팀 자체에서 기반 이미지 자체를 관리하고 싶어지는 순간이 온다고 한다. 팀만의 특정한 규칙 (환경변수라던가 인증서 등등)을 기반 이미지에 추가하고 그것을 **골든 이미지**라고 부른다.

`LABEL` 을 사용하여 골든 이미지의 메타데이터를 정의하고 설정을 정의하는 단순한 이미지를 만들고 이것을 추후 기반 이미지로 사용하면 될 뿐이다.

```dockerfile

```