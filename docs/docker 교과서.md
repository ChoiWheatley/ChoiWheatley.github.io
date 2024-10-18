---
aliases: 
tags:
  - book
description: 
title: docker 교과서
created: 2024-08-31T21:53:58
updated: 2024-10-18T20:15:27
---

## 1부 도커 컨테이너와 이미지 이해하기

[[docker 교과서 Chapter 2]] 도커의 기본적인 사용법

- **Docker Virtualization**: Provides OS-level virtualization for running applications on a virtualized Linux kernel.
- **Docker Container**: Isolates applications with minimal resources like filesystem, IP address, and hostname.
- **Docker Engine**: Manages containers via Docker API, with external command access through REST API.
- **Docker CLI**: A client that interacts with Docker Engine using Docker API.
- **`docker container run`**: Runs a container from an image, downloads it if not found locally.
- **`--interactive/--tty` (`-i/-t`)**: Opens an interactive terminal session in the container.
- **`--detach` (`-d`)**: Runs the container in the background.
- **`--publish`**: Maps host port to container port (e.g., `8080:80`).
- **`docker container ls`**: Lists running containers; `-a` lists all, including stopped ones.
- **`docker container top`**: Displays active processes in a container.
- **`docker container logs`**: Shows container's output logs.
- **`docker container rm`**: Removes containers; `-f` force-removes all.
- **`docker container rmi`**: Deletes images, independent of container removal.
- **`docker container exec`**: Executes commands in a running container, with options like `-it` for shell access.
- **Docker API**: Interfaces with Docker Engine via REST API; CLI is one client, enabling remote management and integration with GUI tools.

[[docker 교과서 Chapter 3]] 도커 이미지 만들기

- **Docker Hub Images**: Use pre-shared images from Docker Hub for easier setup.
- **Writing Dockerfile**: Dockerfile scripts define instructions to build Docker images.
- **Building Container Images**: Compile the image using Dockerfile commands.
- **Understanding Docker Image Layers**: Each Dockerfile command creates a new image layer.
- **Optimizing Dockerfile with Image Layer Cache**: Utilize cached layers to speed up builds by rearranging frequently changed layers last.
- **Docker Run `-e/--env` Flag**: Inject environment variables at container startup.
- **Why Dockerfile?**: Script to package applications into Docker images.
- **Docker Image History**: Inspect image layers using `docker image history`.
- **Docker Image Layer Sharing**: Reuse base layers across images to save space.
- **Docker Layer Caching**: Optimize builds by placing static instructions early in Dockerfile and variable ones later.
- **Manual Container Creation**: Use `docker container commit` to manually create images from container changes.

[[docker 교과서 Chapter 4]] 애플리케이션 소스 코드에서 도커 이미지까지

- **Docker as Build Tool**: Standardizes build environments by defining toolchains (e.g., compilers, linkers) in Dockerfiles.
- **Staging in Docker**: Manages stages (build, test) with separate base images and uses `AS` keyword for stage aliasing.
- **RUN Instruction**: Executes commands inside the container using shell scripts.
- **CMD vs ENTRYPOINT**: CMD for overridable commands, ENTRYPOINT for fixed commands that always run.
- **Docker Virtual Network**: Enables container communication via virtual networks using IPs or container names.
- **Multi-Stage Build**: Reduces image size by separating build tools and runtime; excludes unnecessary tools in final image.
- **Optimized Dockerfile**: Moves frequently changing files (e.g., `index.html`) to later stages and uses multi-stage builds to minimize image size.
- **CI Integration**: Automates building and deploying Docker images with source code using CI pipelines.

[[docker 교과서 Chapter 5]]  도커 허브 등 레지스트리에 이미지 공유하기  
[[docker 교과서 Chapter 6]]  도커 볼륨을 이용한 퍼시스턴트 스토리지

## 2부 컨테이너로 분산 애플리케이션 실행하기

[[docker 교과서 Chapter 7]]  도커 컴포즈로 분산 애플리케이션 실행하기  
[[docker 교과서 Chapter 8]]  헬스 체크와 디펜던시 체크로 애플리케이션 신뢰성 확보하기  
[[docker 교과서 Chapter 9]]  컨테이너 모니터링으로 투명성 있는 애플리케이션 만들기  
[[docker 교과서 Chapter 10]]  도커 컴포즈를 이용한 여러 환경 구성  
[[docker 교과서 Chapter 11]]  도커와 도커 컴포즈를 이용한 애플리케이션 빌드 및 테스트

## 3부 컨테이너 오케스트레이션을 이용한 스케일링

[[docker 교과서 Chapter 12]]  컨테이너 오케스틀이션: 도커 스웜과 쿠버네티스  
[[docker 교과서 Chapter 13]]  도커 스웜 스택으로 분산 애플리케이션 배포하기  
[[docker 교과서 Chapter 14]]  업그레이드와 롤백을 이용한 업데이트 자동화  
[[docker 교과서 Chapter 15]]  보안 원격 접근 및 CI/CD를 위한 도커 설정  
[[docker 교과서 Chapter 16]]  어디서든 실행할 수 있는 도커 이미지 만들기: 리눅스, 윈도, 인텔, ARM

## 4부 운영 환경 투입을 위한 컨테이너 준비하기

[[docker 교과서 Chapter 17]]  도커 이미지 최적화하기: 보안, 용량, 속도  
[[docker 교과서 Chapter 18]]  컨테이너의 애플리케이션 설정 관리  
[[docker 교과서 Chapter 19]]  도커를 이용한 로그 생성 및 관리  
[[docker 교과서 Chapter 20]]  리버스 프록시를 이용해 컨테이너 HTTP 트래픽 제어하기  
[[docker 교과서 Chapter 21]] 메시지 큐를 이용한 비동기 통신  
[[docker 교과서 Chapter 22]] 끝없는 정진
