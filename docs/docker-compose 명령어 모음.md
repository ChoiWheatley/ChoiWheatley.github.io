---
aliases: 
tags: 
description:
created: 2023-07-02T00:24:24
updated: 2023-07-11T15:21:08
title: docker-compose 명령어 모음
---
- [sof](https://stackoverflow.com/questions/36884991/how-to-rebuild-docker-container-in-docker-compose-yml)

```shell
docker-compose up -d # run all services in the background
docker-compose stop nginx # stop only one. but it is still running !!!
docker-compose build --no-cache nginx 
docker-compose up -d --no-deps # link nginx to other services

```

```
Options:
    -d, --detach        Detached mode: Run containers in the background,
                        print new container names. Incompatible with
                        --abort-on-container-exit.
    --no-deps           Don't start linked services.
    --force-recreate    Recreate containers even if their configuration
                        and image haven't changed.
    --build             Build images before starting containers.
```