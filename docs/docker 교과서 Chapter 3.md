---
aliases: 
tags: 
description:
title: docker 교과서 Chapter 3
created: 2024-09-01T19:13:24
updated: 2024-10-07T17:17:18
---

## 목록

1. 도커 허브에 공유된 이미지 사용하기
2. Dockerfile 작성하기
3. 컨테이너 이미지 빌드하기
4. 도커 이미지와 이미지 레이어 이해하기
5. 이미지 레이어 캐시를 이용한 Dockefile 스크립트 최적화

## Docker Run 할때 `-e` 또는 `--env` 플래그

> environment variable을 컨테이너 시작시 넣어줄 수도 있다. 

```
docker container run --name <name> --env [<key>=<value>] diamol/ch03-web-ping
```

## Why Dockerfile?

> dockerfile 작성을 해야 하는 이유는 뭐지?

dockerfile → docker image , 즉 애플리케이션을 패키징(빌드) 하기 위한 스크립트 파일을 의미.

- `FROM`
- `ENV`
- `WORKDIR`
- `COPY`
- `CMD`

## Docker Image Layer

> docker image layer라는게 있다고? dockerfile 하나가 이미지 레이어 하나를 차지한다고? `docker image history` 명령어를 사용하면 내가 빌드한 이미지의 레이어를 출력해준다.

도커파일의 각 명령어 한줄마다 이미지 레이어가 한 줄씩 쌓인다. 아래는 `docker image history web-ping` 명령을 사용하여 실습 중 빌드한 이미지의 레이어를 복기한다:

```
$ docker image history web-ping
IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
218b62931576   5 minutes ago   /bin/sh -c #(nop)  CMD ["node" "/web-ping/ap…   0B
5f46ff96c655   5 minutes ago   /bin/sh -c #(nop) COPY file:a2087c9a71052e4e…   846B
0184d8697d3c   5 minutes ago   /bin/sh -c #(nop) WORKDIR /web-ping             0B
9f258f86775d   5 minutes ago   /bin/sh -c #(nop)  ENV INTERVAL=3000            0B
b3d33740b138   5 minutes ago   /bin/sh -c #(nop)  ENV METHOD=HEAD              0B
5b1ecca41c3f   5 minutes ago   /bin/sh -c #(nop)  ENV TARGET=blog.sixeyed.c…   0B
9dfa73010b19   5 years ago     /bin/sh -c #(nop)  CMD ["node"]                 0B
<missing>      5 years ago     /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B
<missing>      5 years ago     /bin/sh -c #(nop) COPY file:238737301d473041…   116B
<missing>      5 years ago     /bin/sh -c apk add --no-cache --virtual .bui…   5.1MB
<missing>      5 years ago     /bin/sh -c #(nop)  ENV YARN_VERSION=1.16.0      0B
<missing>      5 years ago     /bin/sh -c addgroup -g 1000 node     && addu…   64.7MB
<missing>      5 years ago     /bin/sh -c #(nop)  ENV NODE_VERSION=10.16.0     0B
<missing>      5 years ago     /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
<missing>      5 years ago     /bin/sh -c #(nop) ADD file:a86aea1f3a7d68f6a…   5.53MB
```

Dockerfile의 `FROM` 명령이 이미지 레이어 중 가장 큰 사이즈를 차지한다. FROM 은 기반 명령어를 설정하는 것으로, 이 위에 올라온 모든 JS 파일들은 불과 몇 KB에 불과한다. 그러니까 우리는 컨테이너 하나를 만든다고 Node 환경을 새로 올리지 않는다는 것이다.

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

## 도커 이미지 레이어 캐시를 이용한 Dockerfile 스크립트 최적화

app.js 파일 하나만 변경하고 다른 이미지를 하나 생성해보자.

```
$ docker image build -t web-ping:v2  .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon   5.12kB
Step 1/7 : FROM diamol/node
 ---> 9dfa73010b19
Step 2/7 : ENV TARGET="blog.sixeyed.com"
 ---> Using cache
 ---> 5b1ecca41c3f
Step 3/7 : ENV METHOD="HEAD"
 ---> Using cache
 ---> b3d33740b138
Step 4/7 : ENV INTERVAL="3000"
 ---> Using cache
 ---> 9f258f86775d
Step 5/7 : WORKDIR /web-ping
 ---> Using cache
 ---> 0184d8697d3c
Step 6/7 : COPY app.js .
 ---> 1228aaacf7bb
Step 7/7 : CMD ["node", "/web-ping/app.js"]
 ---> Running in 29f74ab575da
 ---> Removed intermediate container 29f74ab575da
 ---> fba79c20e7aa
Successfully built fba79c20e7aa
Successfully tagged web-ping:v2
```

Step 2,3,4,5 단계가 모두 cache를 사용했음을 알 수 있다. Step 6,7 단계는 새로운 레이어가 만들어졌다. CMD 인스트럭션이 변경되었기 때문에 캐시 미스가 발생했고, 그 이후 단계를 모두 새 레이어에서 실행된다.

> [!note] Dockefile은 명령이 순서대로 실행되기 위해서 존재하지 않는다. 레이어의 순서를 보장하기 위하여 존재한다.

> [!question] 레이어를 최대한 많이 공유하기 위해선 어떻게 해야하나요?

Dockerfile 인스트럭션 중 자주 변경되지 않는 명령을 앞으로 배치하고 자주 수정되는 인스트럭션을 뒤에 배치하면 됩니다. 이미지 레이어는 수정이 발생한 시점부터 분기하게 되는데, 그 뒤에 인스트럭션이 동일하더라도 이전 인스트럭션에서 차이가 발생하면 그 위 레이어들은 캐시를 참조하지 않기 때문입니다.

### 문제: **아래의 Dockerfile을 최적화하세요**

```Dockerfile
FROM diamol/node

ENV TARGET="blog.sixeyed.com"
ENV METHOD="HEAD"
ENV INTERVAL="3000"

WORKDIR /web-ping
COPY app.js .

CMD ["node", "/web-ping/app.js"]
```

1. COPY 인스트럭션이 자주 바뀔 수 있기 때문에 가장 높은 레이어로 옮긴다. (코드의 마지막으로)
2. CMD 인스트럭션은 거의 바뀌지 않을 것이기 때문에 FROM 뒤에 초반부에 배치하자.
3. ENV는 한 번에 여러개를 정의할 수 있기 때문에 레이어 개수를 줄인다.

```Dockerfile
FROM diamol/node

CMD ["node", "/web-ping/app.js"]

ENV TARGET="blog.sixeyed.com"
ENV METHOD="HEAD"
ENV INTERVAL="3000"

WORKDIR /web-ping
COPY app.js .
```

## 3.6 연습문제

> Dockerfile을 사용하지 않고 수동으로 컨테이너를 만들어보세요.

알아야 할 명령어

- `docker container commit`: Create a new image from a container's changes
- `docker container run -it`
- 
