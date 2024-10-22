---
aliases: 
tags: 
description:
title: docker 교과서 Chapter 6
created: 2024-10-22T15:43:22
updated: 2024-10-22T17:47:12
---
**참고자료**
- <https://docs.docker.com/engine/storage/>
- <https://docs.docker.com/engine/storage/volumes/>
- <https://docs.docker.com/engine/storage/bind-mounts/>  

모든 컨테이너는 읽기 전용 이미지 레이어를 공유한다. 쓰기 행위가 컨테이너 안에서 일어나면 COW 메커니즘에 의해 기록 가능 레이어로 파일을 복사해 쓰기를 수행하며, 이는 같은 이미지의 다른 컨테이너와 공유되지 않는다.

컨테이너가 삭제되면 컨테이너 내 쓰기 레이어는 소실된다❗️⇒ Docker Volume / Mount takes in

## Docker volume

도커 볼륨은 컨테이너가 스토리지를 공유할 수 있도록 만들어주는 장치이다. 후에 나올 바인드에 비해 운영체제와는 독립적으로 관리가 되어 모든 환경에서 똑같이 동작한다. 먼저 도커 볼륨을 생성한 뒤에 할당해주어야 사용가능하다.

```
$ docker volume create <volume-name>
$ docker run --volumes-from <volume-name>
```

> [!question] Dockerfile의 `VOLUME` 명령과 docker-cli의 `--volume` 명령의 차이점은 무엇인가요?

문법에서 차이가 있다. `VOLUME <target-directory>` 로, 오직 볼륨을 등록할 컨테이너의 위치만 정의하는 반면에, `--volume <[host-directory|docker-volume]>:<target-directory>[:<permission>]` 처럼 구체적으로 설정할 수 있다.

만약 VOLUME은 정의했는데 --volume을 정의하지 않았다면 도커는 자동으로 해시값으로 도커 볼륨을 하나 만들어준다. 그러니 웬만하면 이름을 지어서 컨테이너끼리 같은 VOLUME을 공유하도록 하는 것이 중요하다.

**cli 상에서 volume 사용법**

```sh
# --volume
docker run -d \
  --name devtest \
  -v myvol2:/app \
  nginx:latest

# --mount
docker run -d \
  --name devtest \
  --mount source=myvol2,target=/app \
  nginx:latest
```

## Docker bind mount

`--volume`, `--mount` 옵션 둘 다 도커 볼륨을 사용할 수도 있고, 도커 마운트를 사용할 수 있다는 점을 미리 알아두고 가자. 참고: <https://docs.docker.com/engine/storage/>

도커 마운트는 호스트 파일 시스템을 컨테이너에 마운트 하는 기능이다. 이 기능을 사용하면 굳이 `docker volume create`를 할 필요 없이 컨테이너 내에서 호스트 파일 시스템에 접근할 수 있게 된다. 보안을 위해 기본적으로는 read only 권한만 존재하며, 이를 풀어주기 위해서는 Dockerfile에서 USER 명령을 사용해서 마운트 지점에서의 사용자 권한을 확보해야 한다.

UNIX 시스템에서 네트워크나 호스트가 마운트한 장치들도 파일로써 간주되므로, 분산 스토리지를 여러 컨테이너가 연결하여 구축하는 것도 가능하다!

**cli 상에서 bind mount 사용법**

```bash
# --volume
$ docker run -d \
  -it \
  --name devtest \
  -v "$(pwd)"/target:/app \
  nginx:latest

# --mount
$ docker run -d \
  -it \
  --name devtest \
  --mount type=bind,source="$(pwd)"/target,target=/app \
  nginx:latest
```

> [!question] Dockerfile이나 일반 이미지가 이미 사용하고 있는 경로를 mount하게 되면 어떻게 되나요?

- *case* Dockerfile에 정의된 이미지 레이어가 이미 해당 경로를 정의한 경우 ⇒ docker-cli 옵션이 경로를 덮어씌워버리게 됩니다.
- *case* 컨테이너에 이미 존재하는 디렉터리로 마운트 하는 경우 ⇒ 
	- *case* Linux ⇒ ~~디렉터리의 파일이 합쳐져 두 파일이 동시에 나타나게 됩니다.~~ 아니잖아 그냥 덮어씌워지잖아
	- *case* Windows ⇒ ⚠️ 확인필요

> bind는 호스트 파일시스템에 의존하고 volume은 도커가 관리합니다. [docs.docker.com#Mount into a non-empty directory on the container](https://docs.docker.com/engine/storage/bind-mounts/#mount-into-a-non-empty-directory-on-the-container)

> 리눅스는 단일 파일 마운트가 가능하지만 윈도우는 ❌ 😓
