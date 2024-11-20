---
aliases: 
tags: 
description:
title: docker --filter flag
created: 2024-11-21T00:08:02
updated: 2024-11-21T00:08:59
---

 > tell me about docker ps --filter queries

The docker ps --filter command allows you to filter the list of running containers based on specific criteria. Here are some common filter options:

- status=<status>: Filter by container status (e.g., running, exited).
- name=<name>: Filter by container name or a part of it.
- id=<container_id>: Filter by container ID.
- label=<key=value>: Filter by container labels.
- ancestor=<image>: Filter by the image used to create the container.

**Example usage:**

```
docker ps --filter "status=running"  
docker ps --filter "name=my_container"
```

This helps in managing and monitoring containers more effectively.  
➜  Giftogether_backend git:(main) ✗ sgpt "how to change view for docker ps? I want to see only CONTAINER ID, NAME column"  
You can customize the output of docker ps using the --format option. To display only the CONTAINER ID and NAME columns, use the following command:


 docker ps --format "table {{.ID}}\t{{.Names}}"


This command formats the output as a table with the specified columns. If you want a simpler output without the table format, you can use:


 docker ps --format "{{.ID}} {{.Names}}"
