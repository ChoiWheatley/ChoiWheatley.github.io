---
aliases: 
tags: 
description:
title: postgresql on docker
created: 2023-11-13T16:11:15
updated: 2023-11-13T16:13:10
---
- <https://hub.docker.com/_/postgres>
- [[0018.1 Nest.js 🪺]]
---

## simple commands

 To interact with the PostgreSQL shell using Docker, you can run a PostgreSQL container and access its shell. Here's a basic example using the `postgres` official Docker image:

1. Pull the PostgreSQL image:

```bash
docker pull postgres
```

1. Run a PostgreSQL container:

```bash
docker run --name mypostgres -e POSTGRES_PASSWORD=mysecretpassword -p 5432:5432 -d postgres
```

Replace `mypostgres` with your desired container name and `mysecretpassword` with your preferred password.

1. Access the PostgreSQL shell in the running container:

```bash
docker exec -it mypostgres psql -U postgres
```

This command opens an interactive terminal (`-it`) and connects to the PostgreSQL server as the default user (`postgres`).

Now, you can execute SQL commands in the PostgreSQL shell. Don't forget to replace `mypostgres` with your actual container name.

If you have a specific use case or question about interacting with the PostgreSQL shell, feel free to provide more details.
