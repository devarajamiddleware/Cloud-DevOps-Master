
# Day 1 - Docker Basics

## Docker Images

A Docker image is a read-only template used to create containers.

Examples:
- hello-world
- ubuntu
- nginx
- redis
- mysql

Command:

```bash
docker images
```

Important Columns:

- Repository – Image name
- Tag – Version
- Image ID – Unique identifier

## Running Ubuntu Container

Command:

```bash
docker run -it ubuntu bash
```

Explanation:

- docker → Docker CLI
- run → Create a new container
- -i → Interactive mode
- -t → Allocate a terminal
- ubuntu → Image name
- bash → Start Bash shell

Prompt:

root@<container-id>:/#

Meaning:

- root → Logged-in user
- container-id → Unique container identifier
- / → Root directory
