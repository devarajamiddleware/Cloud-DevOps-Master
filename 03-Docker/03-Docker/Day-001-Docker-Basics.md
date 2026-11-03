# Day 1 - Docker Fundamentals

**Date:** 04-Nov-2026

**Course:** Azure Cloud & DevOps Master

**Trainer:** ChatGPT

---

# Objectives

Today I learned:

- Why Docker was created
- Docker Image
- Docker Container
- Docker Hub
- Docker Architecture
- Image vs Container
- Container Lifecycle
- Running Ubuntu Container
- Basic Linux inside Docker
- Difference between `docker run` and `docker start`
- Difference between `docker ps` and `docker ps -a`

---

# Why Docker?

Before Docker, developers used to say:

> "It works on my machine."

Applications worked on the developer's laptop but failed in Test, UAT, or Production because the environments were different.

Example:

Developer Machine

- Ubuntu 22
- Java 17
- Tomcat 10

Production Server

- RHEL 8
- Java 11
- Tomcat 9

Docker solves this problem by packaging:

- Application
- Runtime
- Libraries
- Dependencies
- Configuration

into a Docker Image.

---

# What is Docker Image?

A Docker Image is a **read-only template** used to create Docker Containers.

Examples:

- ubuntu
- nginx
- redis
- mysql
- tomcat
- hello-world

Command:

```bash
docker images
```

Example Output

```
REPOSITORY        TAG      IMAGE ID
ubuntu            latest   xxxxxxxxx
hello-world       latest   xxxxxxxxx
```

---

# Docker Image Components

## Repository

Image name.

Example:

```
ubuntu
```

---

## Tag

Represents version.

Examples

```
latest
22.04
24.04
```

---

## Image ID

Unique identifier of Docker Image.

Example

```
c567b231f656
```

---

# Interview Question

### What is Docker Image?

**Answer**

A Docker Image is a read-only template containing application code, runtime, libraries, dependencies and configuration used to create Docker Containers.

---

# Docker Hub

Docker Hub is a public registry used to download Docker Images.

Docker Workflow

```
docker run ubuntu
        │
        ▼
Check Local Image
        │
   Yes ─────────► Use Local Image
        │
   No
        ▼
Pull Image from Docker Hub
        │
        ▼
Create Container
```

---

# docker run

Command

```bash
docker run -it ubuntu bash
```

Explanation

| Option | Meaning |
|---------|----------|
| docker | Docker CLI |
| run | Create new container |
| -i | Interactive mode |
| -t | Allocate terminal |
| ubuntu | Image |
| bash | Start Bash Shell |

---

# Internally Docker Performs

```
Docker Hub
     │
Download Image
     │
Create Container
     │
Start Container
     │
Launch Bash
```

---

# Linux Prompt

```
root@8a12ce422f26:/#
```

Meaning

| Item | Meaning |
|------|---------|
| root | Logged-in User |
| 8a12ce422f26 | Container ID |
| / | Linux Root Directory |

---

# Image vs Container

Image = Blueprint

Container = Running Instance

Example

```
Ubuntu Image
      │
 ┌────┼─────┐
 │    │     │
 ▼    ▼     ▼
Container1
Container2
Container3
```

One Image can create multiple Containers.

---

# Docker Architecture

```
Docker CLI
      │
      ▼
Docker Engine
      │
 ┌──────────────┐
 │ Images       │
 │ Containers   │
 │ Networks     │
 │ Volumes      │
 └──────────────┘
      │
      ▼
Docker Hub
```

---

# Commands Practiced

## List Images

```bash
docker images
```

---

## Run Ubuntu Container

```bash
docker run -it ubuntu bash
```

---

## Check Linux Version

```bash
cat /etc/os-release
```

---

## Print Current Directory

```bash
pwd
```

---

## Stop Container

```bash
docker stop <container-id>
```

Example

```bash
docker stop 8a12ce422f26
```

---

## Create Named Container

```bash
docker run -it --name devops-lab ubuntu bash
```

---

## Exit Container

```bash
exit
```

---

## Running Containers

```bash
docker ps
```

---

## All Containers

```bash
docker ps -a
```

---

## Start Existing Container

```bash
docker start devops-lab
```

---

# Difference Between docker run and docker start

## docker run

- Creates NEW container
- Starts NEW container
- Pulls Image if unavailable

---

## docker start

- Starts EXISTING stopped container
- Does NOT create container

---

# Difference Between docker ps and docker ps -a

## docker ps

Shows only Running Containers.

## docker ps -a

Shows

- Running
- Stopped
- Exited
- Created

Containers.

---

# Container Lifecycle

```
docker run
     │
Created
     │
Running
     │
docker stop
     │
Exited
     │
docker start
     │
Running
     │
docker rm
     │
Deleted
```

---

# Naming Containers

Random Name

```
zealous_poitras
```

Custom Name

```bash
docker run --name devops-lab ubuntu
```

Container Name

✅ Can be changed.

Container ID

❌ Cannot be changed.

---

# Production Example

Instead of

```
docker stop 8a12ce422f26
```

Use

```
docker stop devops-lab
```

Much easier to remember.

---

# Linux Commands Learned

## pwd

Print Working Directory

```bash
pwd
```

Output

```
/
```

---

## cat

Display File Content

```bash
cat /etc/os-release
```

---

## exit

Exit Current Shell

```bash
exit
```

---

# Docker vs Virtual Machine

| Virtual Machine | Docker |
|----------------|---------|
| Own OS | Shares Host OS Kernel |
| Large Size | Lightweight |
| Slow Boot | Fast Startup |
| High RAM Usage | Low RAM Usage |
| More CPU | Less CPU |

---

# Interview Questions

## Why Docker?

To solve the "Works on my machine" problem.

---

## What is Docker Image?

Read-only template.

---

## What is Docker Container?

Running instance of Image.

---

## What is Docker Tag?

Represents Image Version.

Example

```
ubuntu:24.04
```

---

## Why didn't Docker download Ubuntu the second time?

Because Docker first checks the local image cache.

If image exists locally,

Docker reuses it.

---

## Why is Docker faster than Virtual Machines?

Containers share the Host OS Kernel.

No need to boot an Operating System for every application.

---

## Difference Between Image and Container

Image = Blueprint

Container = Running Instance

---

## What happened after executing

```bash
docker run -it ubuntu bash
```

Docker:

1. Check Local Image
2. Pull Image if unavailable
3. Create Container
4. Start Container
5. Start Bash Shell

---

## Difference Between docker run and docker start

docker run

Creates New Container.

docker start

Starts Existing Container.

---

## Difference Between docker ps and docker ps -a

docker ps

Running Containers.

docker ps -a

All Containers.

---

## Can we change Container ID?

No.

Container ID is generated by Docker.

---

## Can we change Container Name?

Yes.

```bash
docker rename old-name new-name
```

---

## What happens if we execute

```bash
docker run --name devops-lab ubuntu
```

again?

Docker returns

```
Conflict.
Container name already exists.
```

Container Names must be unique.

---

# Commands to Remember

```bash
docker images
docker run -it ubuntu bash
docker run -it --name devops-lab ubuntu bash
docker ps
docker ps -a
docker start devops-lab
docker stop devops-lab
docker rm devops-lab
docker rename old-name new-name
pwd
cat /etc/os-release
exit
```

---

# Homework

- Revise today's concepts.
- Explain Image vs Container in your own words.
- Explain docker run vs docker start.
- Practice creating, stopping, starting and listing containers.

---

# Day 2 Preview

- docker exec
- docker logs
- docker inspect
- docker rm
- docker rmi
- Docker Volumes
- Docker Networks
- Run Nginx Container
- Access Application from Browser
- Real Production Troubleshooting
