---
aliases: 
tags: 
description:
title: docker 교과서 Chapter 7
created: 2024-10-22T18:09:01
updated: 2024-10-22T19:39:38
---
docker-compose는 여러 컨테이너를 하나의 호스트에서 관리할 수 있게 도와준다. docker-cli만 가지고선 이미지를 빌드하는 방법에 대한 별도의 문서를 마련해야 했지만, docker-compose가 선언형으로 컨테이너들을 실행하는데 필요한 단계와 속성을 정의하여 편해졌다.

*docker-compose.yml 파일 예제*

```yml
version: '3.7'

services:

  accesslog:
    image: diamol/ch04-access-log
    networks:
      - app-net

  iotd:
    image: diamol/ch04-image-of-the-day
    ports:
      - "80"
    networks:
      - app-net

  image-gallery:
    image: diamol/ch04-image-gallery
    ports:
      - "8010:80" 
    depends_on:
      - accesslog
      - iotd
    networks:
      - app-net

networks:
  app-net:
    external:
      name: nat
```

- `version`: docker-compose 자체의 버전을 명시한다.
- `services`: container와 같은 개념. 위의 예제는 하나의 컨테이너만을 정의하는 예제이다.
- `networks`: 도커 네트워크를 정의. `external`을 사용하면 자동생성을 방지한다.
- `depends_on`: 컨테이너 실행 순서를 정의한다. 즉, 아래에 나열된 원소들이 먼저 실행이 된 다음에 자신의 서비스가 실행이 된다.

## docker-compose CLI cheatsheet

[[docker-compose 명령어 모음]]

**scale** 옵션은 동일한 컨테이너를 스케일 아웃하는데 사용된다. 스케일 아웃된 컨테이너에 요청이 들어오면 도커컴포즈가 이븐하게 요청을 나눠주어 부하를 막아준다. 

도커는 자체적으로 DNS 기능까지 포함하고 있는데, 컨테이너 이름 (`--name` 옵션)이 DNS 테이블에 포함되어 있는 것을 확인할 수 있다.

```
$ docker exec -it image-of-the-day-image-gallery-1 sh
/web # nslookup iotd
Name:      iotd
Address 1: 172.18.0.4 image-of-the-day-iotd-3.nat
Address 2: 172.18.0.3 image-of-the-day-iotd-2.nat
Address 3: 172.18.0.5 image-of-the-day-iotd-1.nat
```

## docker-compose의 한계

단일 서버에서의 컨테이너들을 관리하기 때문에 분산 서비스 아키텍처를 계획중이라면 도커 스웜이나 쿠버네티스와 같은 플랫폼을 사용해야 한다. 하지만 배포환경이 그렇게 크지 않거나 CI, 테스팅을 위한 간소한 환경을 구축하고자 한다면 docker-compose는 좋은 선택이다.

[[docker-compose의 한계, 오케스트레이션의 등장]]
