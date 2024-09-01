---
aliases: 
tags: 
description:
title: docker 교과서 Chapter 3
created: 2024-09-01T19:13:24
updated: 2024-09-01T20:52:14
---

## 목록

1. 도커 허브에 공유된 이미지 사용하기
2. Dockerfile 작성하기
3. 컨테이너 이미지 빌드하기
4. 도커 이미지와 이미지 레이어 이해하기
5. 이미지 레이어 캐시를 이용한 Dockefile 스크립트 최적화

## 끄적이기

> environment variable을 컨테이너 시작시 넣어줄 수도 있다. 

```
docker container run --name <name> --env [<key>=<value>] diamol/ch03-web-ping
```

> dockerfile 작성을 해야 하는 이유는 뭐지?

dockerfile → docker image , 즉 애플리케이션을 패키징(빌드) 하기 위한 스크립트 파일을 의미.

- `FROM`
- `ENV`
- `WORKDIR`
- `COPY`
- `CMD`

> docker image layer라는게 있다고? dockerfile 하나가 이미지 레이어 하나를 차지한다고? `docker image history` 명령어를 사용하면 내가 빌드한 이미지의 레이어를 출력해준다.

```
$ docker image ls
REPOSITORY             TAG       IMAGE ID       CREATED          SIZE
web-ping               latest    424363da0993   21 minutes ago   75.5MB
diamol/ch03-web-ping   latest    bfce5d697312   3 years ago      75.5MB
```

여기에서 SIZE가 75.5MB씩 두개가 되는데 실제로 차지하는 용량을 확인해보면...

```
$ docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          2         2         75.49MB   75.49MB (99%)
Containers      2         0         0B        0B
Local Volumes   2         0         50.11MB   50.11MB (100%)
Build Cache     6         0         0B        0B
```

`TYPE IMAGES` 쪽만 보면 되는데, 이미지에 75.49MB만을 사용하고 있는 것으로 알 수 있다. 즉 같은 레이어를 공유하고 있다고 볼 수 있다. 만약 다른 node.js 이미지를 빌드한다면 최상위 레이어 이미지의 사이즈만 추가될 것이다. 멋진 말로 **"같은 기반 레이어를 공유하는 애플리케이션의 숫자가 많을 수록 절약된다."**

> [!question] 레이어를 최대한 많이 공유하기 위해선 어떻게 해야하나요?

Dockerfile 인스트럭션 중 자주 변경되지 않는 명령을 앞으로 배치하고 자주 수정되는 인스트럭션을 뒤에 배치하면 됩니다. 이미지 레이어는 수정이 발생한 시점부터 분기하게 되는데, 그 뒤에 인스트럭션이 동일하더라도 이전 인스트럭션에서 차이가 발생하면 그 위 레이어들은 캐시를 참조하지 않기 때문입니다.

도커 이미지를 빌드할때 
