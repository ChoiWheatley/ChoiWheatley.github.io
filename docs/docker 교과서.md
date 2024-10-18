---
aliases: 
tags:
  - book
description: 
title: docker 교과서
created: 2024-08-31T21:53:58
updated: 2024-10-18T19:48:06
---
[[docker 교과서 Chapter 2]]

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

[[docker 교과서 Chapter 3]]

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

[[docker 교과서 Chapter 4]]

- **Docker as Build Tool**: Standardizes build environments by defining toolchains (e.g., compilers, linkers) in Dockerfiles.
- **Staging in Docker**: Manages stages (build, test) with separate base images and uses `AS` keyword for stage aliasing.
- **RUN Instruction**: Executes commands inside the container using shell scripts.
- **CMD vs ENTRYPOINT**: CMD for overridable commands, ENTRYPOINT for fixed commands that always run.
- **Docker Virtual Network**: Enables container communication via virtual networks using IPs or container names.
- **Multi-Stage Build**: Reduces image size by separating build tools and runtime; excludes unnecessary tools in final image.
- **Optimized Dockerfile**: Moves frequently changing files (e.g., `index.html`) to later stages and uses multi-stage builds to minimize image size.
- **CI Integration**: Automates building and deploying Docker images with source code using CI pipelines.
