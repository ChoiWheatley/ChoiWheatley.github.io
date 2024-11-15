---
aliases: 
tags:
  - book
description: 
title: docker 교과서
created: 2024-08-31T21:53:58
updated: 2024-11-15T18:03:49
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

- **Docker Image Naming**: Docker images are identified as `registry/group/image:tag` (e.g., `docker.io/diamol/base:latest`).
- **Docker Tag**: Use `docker tag` to assign names and tags to images, allowing multiple names for the same image ID.
- **Docker Image Push**: Use `docker image push` to upload images to a registry (e.g., Docker Hub), with login via `docker login`.
- **Docker Registry as Image**: A Docker registry can also be run as a container and used for local image storage and backups.
- **Tag Naming Convention**: Follows semantic versioning (`Major.Minor.Patch`), where specific or broader tags can be used (e.g., `hello:1`, `hello:1.2`, `hello:latest`).
- **Golden Image**: A customized base image with team-specific configurations, often created using `LABEL` for metadata.
- **Docker API for Registry**: Manage and verify images using the Docker registry API with requests like `GET /v2/_catalog`, `GET /v2/<name>/tags/list`, and `DELETE /v2/<name>/manifests/<digest>`.

[[docker 교과서 Chapter 6]]  도커 볼륨을 이용한 퍼시스턴트 스토리지

- **COW Mechanism**: Containers share read-only image layers, and changes are written to a unique writable layer (not shared between containers).
- **Data Persistence**: Container deletion removes the writable layer; Docker volumes or mounts provide persistent storage.

- **Docker Volume**: A Docker-managed storage solution that allows containers to share data, independent of the host OS. Volumes are consistent across environments.
    - **Creation**: Use `docker volume create` to create a volume, and `--volume` or `--mount` to attach it to a container.
    - **VOLUME vs --volume**: Dockerfile `VOLUME` defines the container’s storage location; `--volume` specifies both the source and target, allowing for more control.

- **Docker Bind Mount**: Allows direct mounting of host filesystem directories or files into a container without needing to create a Docker volume.
    - **Usage**: Bind mount with `--volume` or `--mount` options. 
    - **File Access**: Default permissions are read-only for security; modify with the `USER` command in Dockerfile to manage access.
    - **Host Dependence**: Bind mounts rely on the host’s file system, unlike volumes, which Docker manages independently.
    - **Overwriting Paths**: Mounting into an existing directory replaces the container's files at that path with the host's files.

- **Volume vs Bind Mount**: 
    - Volumes are managed by Docker and portable across environments.
    - Bind mounts rely on host-specific directories, making them less portable but providing direct access to host files.

- **Linux vs Windows**: Linux allows for mounting both directories and single files, while Windows does not support single-file bind mounts.

## 2부 컨테이너로 분산 애플리케이션 실행하기

[[docker 교과서 Chapter 7]]  도커 컴포즈로 분산 애플리케이션 실행하기  

- **Docker Compose**: A tool that simplifies managing multiple containers on a single host by defining services and networks in a declarative `docker-compose.yml` file.
- **`version`**: Specifies the version of Docker Compose being used.
- **`services`**: Defines containers (e.g., `accesslog`, `iotd`, `image-gallery`), each of which can have attributes like images, ports, networks, etc.
- **`networks`**: Defines the network configuration for containers, with options like `external` to use an existing network instead of creating a new one.
- **`depends_on`**: Specifies the order of container startup, ensuring dependencies start before the current service.
- **`ports`**: Maps ports between the host and the container (e.g., `8010:80`).
- **`volumes` vs `bind mount`**: `volumes` are Docker-managed storage, while `bind mount` links host filesystem directories directly to containers.
- **`restart` Policy**: Ensures containers restart after failures or host reboots (e.g., `always`, `unless-stopped`).
- **`scale`**: Scales out identical containers to distribute load, leveraging Docker's DNS to balance requests.
- **Limitations**: Docker Compose is limited to single-host environments, and for distributed systems, tools like Docker Swarm or Kubernetes are required.
- **Orchestration Alternatives**: Docker Swarm or Kubernetes are used when managing multiple hosts or a more complex service architecture.
- **Secrets Management**: Store sensitive data like passwords or API keys using `secrets` in `docker-compose.yml`, linking them securely to services.
- **Bind Mount vs Volume**: Bind mounts are useful for local development, while Docker volumes are preferred for production due to their independence from the host filesystem.

[[docker 교과서 Chapter 8]]  헬스 체크와 디펜던시 체크로 애플리케이션 신뢰성 확보하기  

- **HEALTHCHECK**: Docker instruction to monitor the internal health of a container by running custom commands at set intervals.
- **Status Monitoring**: Checks for issues like HTTP 500 errors, where the process may be running, but the container is not functioning correctly.
- **`curl --fail`**: A command that returns `0` if the endpoint is accessible and a non-zero exit code if there is an error, helping Docker set the health status to either `healthy` or `unhealthy`.
- **Inspecting Health Logs**: Use `docker container inspect` to view detailed health logs, including `ExitCode` and `Output`, which record error codes and responses.
- **Dependency Check**: Ensures that a service waits for its dependencies (e.g., databases or APIs) to be ready before starting.
- **`depends_on` in Docker Compose**: Defines the startup order of services, though it does not guarantee a service's readiness; best for controlling order, not dependency checks.
- **Direct Dependency Check in Dockerfile**: Uses `CMD` to execute a `curl` request to check if a dependent service is accessible, only starting the application if the dependency is met.
- **Custom Utility for Dependency Check**: Instead of `curl`, custom-built utilities specific to the application are preferred for dependency checks, enhancing security and reducing unnecessary tools in the container.
- **Docker Compose Healthcheck**: Defines healthchecks in `docker-compose.yml` using `interval`, `timeout`, and `retries` to repeatedly check a service’s health.
- **Automatic Restart with `restart: on-failure`**: Configures Docker to restart a service if it fails, useful for dependency checks without `depends_on` by reattempting until dependencies are ready.
- **Restart Limitations in Compose**: Docker Compose can restart containers based on health status but cannot replace an unhealthy container with a new one, unlike advanced orchestration systems.

[[docker 교과서 Chapter 9]]  컨테이너 모니터링으로 투명성 있는 애플리케이션 만들기  

- **Prometheus**: A monitoring tool for collecting, storing, and querying metrics, designed for tracking container health and performance across distributed environments through a standardized API.
- **Monitoring Docker Engine**: Prometheus can monitor Docker Engine metrics directly, tracking data like container states and failed image builds through /metrics at 127.0.0.1:9323.
- **Container-Specific Metrics**: Prometheus requires applications to expose their own metrics via Prometheus libraries, supporting custom metrics like response time, error rates, and specific logging events.
- **Key Metrics**: Recommended metrics include **response time** (e.g., latency between services or user response times) and **simple log counts** (e.g., 500 errors), which provide valuable insights with minimal resource usage.
- **Prometheus in Golang**: Example uses Prometheus Go client library to define Gauge and Counter metrics, exposing these via /metrics.
- **Prometheus in NodeJS**: Uses prom-client to track metrics like total log requests and unique IP addresses, exposing the /metrics endpoint for scraping.
- **Prometheus Graphs**: PromQL is used to query and visualize metric data over time. For example, sum functions aggregate data, and without excludes unwanted attributes.
- **Dynamic Scraping**: Prometheus prometheus.yml configurations allow dynamic scraping of container metrics using dns_sd_configs (for services with multiple IPs) and static_configs (for fixed services).
- **Grafana**: A dashboard tool that integrates with Prometheus to visualize metrics, enhancing application observability. It uses PromQL to pull and display Prometheus metrics in customized, interactive graphs.
- **Service Level Indicators (SLIs)**: Key performance indicators such as latency, availability, and error rates, essential for monitoring application health. Resources like Google’s Site Reliability Engineering (SRE) guide provide guidance on SLIs.

[[docker 교과서 Chapter 10]]  도커 컴포즈를 이용한 여러 환경 구성  

- **Docker Compose for Environments**: Useful in cases where multiple containers need to run on a single Docker Engine, often for smaller services, testing, or development.
- **Project Naming**: Allows running multiple instances of the same Compose setup by changing the project name (e.g., `-p todo-test`), helping avoid conflicts by isolating container names.
- **Port Conflicts**: If containers have predefined ports in their configurations, repeated instances may use random ports. Use `docker port <container-name> <inner-port>` to retrieve active port mappings.
- **Docker Compose Override**: Combines multiple Compose files (`base` and `override`) to modify configuration for specific environments without changing the base file structure.
- **Environment-Specific Overrides**: Set up distinct configurations (e.g., `docker-compose-dev.yml`, `docker-compose-test.yml`) for dev, test, and user acceptance testing, customizing ports, networks, health checks, and restart policies.
- **Environment Variables and Secrets**: 
	- **`environment`**: Defines key-value pairs directly in the Compose file.
    - **`env_file`**: Loads environment variables from a file with shell-style syntax.
    - **`secrets`**: Declares sensitive data, stored securely and mapped to services in the `secrets` section.
- **Using Local Environment Variables**: Use `${VAR}` syntax to pull values from the host environment, allowing flexible variable usage without explicit override files.
- **YAML Extensions (Anchors and Aliases)**: Helps reduce redundancy by defining common settings (`&anchor`) and reusing them (`*alias`) across multiple services.
- **Application Configuration with Docker Compose**: Adjust settings per environment for options like databases, port bindings, networks, and application-specific configs (e.g., logging level, cache size).  
**Use Case Summary**:
   - **Development**: Local database, custom port (`8089`), specific image version (e.g., `v2`).
   - **Testing**: Separate database service (e.g., PostgreSQL), dedicated volume for persistence, assigned port (`8080`), and latest image version.

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
