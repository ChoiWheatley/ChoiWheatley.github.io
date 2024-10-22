---
aliases: 
tags: 
description:
created: 2023-07-02T00:24:24
updated: 2024-10-23T00:05:12
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

### Basic Commands

| Command                               | Description                                                                                |
| ------------------------------------- | ------------------------------------------------------------------------------------------ |
| `docker-compose up`                   | Builds, (re)creates, starts, and attaches to containers.                                   |
| `docker-compose up -d`                | Starts containers in the background (detached mode).                                       |
| `docker-compose down`                 | Stops and removes containers, networks, volumes, and images created by `up`.               |
| `docker-compose start`                | Starts existing services (previously created containers).                                  |
| `docker-compose stop`                 | Stops running services but does not remove containers.                                     |
| `docker-compose restart`              | Restarts services.                                                                         |
| `docker-compose ps`                   | Lists containers for a project.                                                            |
| `docker-compose build`                | Builds or rebuilds the services.                                                           |
| `docker-compose pull`                 | Pulls service images defined in the `docker-compose.yml` file.                             |
| `docker-compose exec <service> <cmd>` | Runs a command inside a running service container. Example: `exec web bash`.               |
| `docker-compose logs`                 | Displays log output from containers.                                                       |
| `docker-compose logs -f`              | Follows log output.                                                                        |
| `docker-compose -f <yml-file> <cmd>`  | Select specific docker-compose script file. ==⚠️ up 할때 -f를 썼다면 다른 모든 명령에도 -f 옵션을 붙여야 한다.== |

---

### Networking & Volumes

| Command                                | Description                                                                   |
| -------------------------------------- | ----------------------------------------------------------------------------- |
| `docker-compose up --force-recreate`   | Forces recreation of containers even if there were no changes.                |
| `docker-compose up --no-deps`          | Starts a service without starting its linked/dependent services.              |
| `docker-compose down --volumes`        | Removes named volumes declared in `volumes` section of `docker-compose.yml`.  |
| `docker-compose down --remove-orphans` | Removes containers for services not defined in the `docker-compose.yml` file. |

---

### Scaling & Service Management

| Command                                       | Description                                                               |
|-----------------------------------------------|---------------------------------------------------------------------------|
| `docker-compose scale <service>=<num>`        | Sets the number of containers for a service. Example: `scale web=3`.      |
| `docker-compose run <service> <cmd>`          | Runs a one-time command on a new container of the service.                |
| `docker-compose stop <service>`               | Stops a specific service.                                                 |
| `docker-compose rm <service>`                 | Removes stopped service containers.                                       |
| `docker-compose pause <service>`              | Pauses a specific service.                                                |
| `docker-compose unpause <service>`            | Unpauses a specific service.                                              |

---

### Configuration & Debugging

| Command                                 | Description                                                                 |
|-----------------------------------------|-----------------------------------------------------------------------------|
| `docker-compose config`                 | Validates and displays the Compose file.                                    |
| `docker-compose config -q`              | Checks if the Compose file is valid (silent on success).                    |
| `docker-compose config --services`      | Lists the services defined in the `docker-compose.yml` file.                |
| `docker-compose config --volumes`       | Lists the volumes defined in the `docker-compose.yml` file.                 |
| `docker-compose version`                | Displays the Docker Compose version information.                            |

---

### Example Commands

```bash
# Start services and build if needed
docker-compose up --build

# Stop and remove containers and volumes
docker-compose down --volumes

# Run a command in a running container (e.g., web service)
docker-compose exec web bash

# View logs for a specific service
docker-compose logs web

# Scale the web service to 3 containers
docker-compose --scale web=3
```
