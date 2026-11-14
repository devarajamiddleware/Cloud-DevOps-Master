# Docker Day 2 - Networking
**Course:** Cloud & DevOps Master Program  
**Student:** Devaraja K.N  
**Day:** 2  
**Topic:** Docker Networking, Port Mapping & Docker DNS

---

# Objectives

By the end of this session I learned:

- Docker Port Mapping
- Docker Bridge Network
- Docker Custom Bridge Network
- Docker DNS
- Container-to-Container Communication
- Docker Network Troubleshooting
- Production Networking Concepts

---

# 1. What is Docker Networking?

Docker networking allows containers to communicate with:

- Other Docker containers
- The Docker Host
- External systems
- Internet

Without networking, containers are isolated.

Example:

```
Internet
     |
Docker Host
     |
-------------------------
|         |             |
Nginx   Backend      MySQL
```

Containers communicate using Docker networks.

---

# 2. Default Docker Networks

Command:

```powershell
docker network ls
```

Output:

```
bridge
host
none
```

### Bridge Network

Default network used for standalone containers.

### Host Network

Container shares host network.

### None Network

Container has no network connectivity.

---

# 3. Docker Port Mapping

Command

```powershell
docker run -d --name nginx-lab -p 8080:80 nginx
```

Meaning

```
HOST PORT : CONTAINER PORT

8080      :       80
```

Diagram

```
Browser

localhost:8080
      |
Docker Host
      |
Docker NAT
      |
Nginx Container
Port 80
```

---

# 4. Two Containers Using Same Internal Port

Container 1

```
8080 -> 80
```

Container 2

```
8081 -> 80
```

Both work successfully because Host Ports are different.

---

# Wrong Example

```
8080 -> 80

8080 -> 80
```

Docker Error

```
port is already allocated
```

Reason

Host port must be unique.

---

# 5. Commands Practiced

## List Docker Networks

```powershell
docker network ls
```

Purpose

Displays available Docker networks.

---

## Inspect Bridge Network

```powershell
docker network inspect bridge
```

Purpose

Shows

- Containers
- Gateway
- Subnet
- IP Addresses

---

## Inspect Container IP

```powershell
docker inspect --format='{{.NetworkSettings.Networks.bridge.IPAddress}}' nginx-lab
```

Output

```
172.17.0.3
```

---

## Create Custom Network

```powershell
docker network create devops-net
```

Purpose

Creates a custom bridge network.

---

## Connect Container

```powershell
docker network connect devops-net nginx-lab
```

---

```powershell
docker network connect devops-net nginx-lab-2
```

Purpose

Connects containers to custom network.

---

## Inspect Custom Network

```powershell
docker network inspect devops-net
```

Shows

- Network ID
- Subnet
- Gateway
- Connected Containers

---

# 6. Custom Network

Created

```
devops-net
```

Subnet

```
172.18.0.0/16
```

Gateway

```
172.18.0.1
```

Containers

```
nginx-lab

172.18.0.2
```

```
nginx-lab-2

172.18.0.3
```

Diagram

```
              devops-net

         172.18.0.0/16

      -----------------------

      nginx-lab

      172.18.0.2

             |

      nginx-lab-2

      172.18.0.3
```

---

# 7. Docker DNS

Instead of using

```
http://172.18.0.2
```

We use

```
http://nginx-lab
```

Docker automatically converts

```
nginx-lab

↓

172.18.0.2
```

Advantages

- IP can change
- Container name remains same
- Easy to maintain
- Best Practice

---

# 8. Testing Docker DNS

Command

```powershell
docker run --rm --network devops-net curlimages/curl -I http://nginx-lab
```

Meaning

| Option | Description |
|---------|-------------|
| docker run | Create container |
| --rm | Remove after execution |
| --network | Attach to network |
| curlimages/curl | Curl image |
| -I | Fetch HTTP Headers |

Output

```
HTTP/1.1 200 OK
```

Second Container

```powershell
docker run --rm --network devops-net curlimages/curl -I http://nginx-lab-2
```

Output

```
HTTP/1.1 200 OK
```

This confirms

- Docker DNS works
- Containers communicate
- Custom network is working

---

# 9. Difference Between Bridge and Custom Bridge

| Bridge | Custom Bridge |
|---------|---------------|
| Default | User Created |
| Limited DNS | Automatic DNS |
| Basic Networking | Recommended for Applications |
| Default Configuration | Better Isolation |

---

# 10. Production Scenario

```
Frontend

↓

Backend

↓

Database
```

All containers communicate using Docker Network.

Never use IP Addresses.

Always use Container Names.

Example

```
backend

↓

mysql-db
```

Not

```
172.18.0.5
```

---

# 11. Troubleshooting Commands

View Networks

```powershell
docker network ls
```

Inspect Network

```powershell
docker network inspect devops-net
```

Inspect Container

```powershell
docker inspect nginx-lab
```

Check Container Status

```powershell
docker ps
```

Check Port Mapping

```powershell
docker port nginx-lab
```

View Logs

```powershell
docker logs nginx-lab
```

Execute Inside Container

```powershell
docker exec -it nginx-lab bash
```

Show Running Processes

```powershell
docker top nginx-lab
```

---

# 12. Common Errors

## Docker Engine Not Running

Error

```
failed to connect to the docker API
```

Solution

Start Docker Desktop.

---

## Network Already Exists

```
network with name devops-net already exists
```

Meaning

Network already created.

---

## Endpoint Already Exists

```
endpoint with name nginx-lab already exists
```

Meaning

Container is already connected.

---

## No Such Container

```
No such container
```

Reason

Container name is incorrect.

Example

Wrong

```
nginx-lab2
```

Correct

```
nginx-lab-2
```

---

# 13. Interview Questions

### Q1

Why do containers need networking?

Answer

To communicate with other containers and external systems.

---

### Q2

Why use Container Names instead of IP?

Answer

Container IP changes.

Container name remains same.

---

### Q3

Difference

```
docker network ls
```

and

```
docker network inspect bridge
```

Answer

network ls

Lists all networks.

inspect

Shows complete network details.

---

### Q4

Difference

```
docker ps
```

and

```
docker network ls
```

Answer

docker ps

Lists containers.

docker network ls

Lists networks.

---

### Q5

Can two containers use

```
8080 -> 80

8081 -> 80
```

Answer

Yes.

Host ports are different.

---

### Q6

Can two containers use

```
8080 -> 80

8080 -> 80
```

Answer

No.

Host port conflict.

---

### Q7

Can containers on different Docker Networks communicate directly?

Answer

No.

They must be connected to the same network.

---

### Q8

What is Docker DNS?

Answer

Docker automatically resolves Container Names into IP Addresses.

---

# Key Takeaways

- Docker networking enables communication between containers.
- Bridge is the default Docker network.
- Custom bridge networks provide automatic DNS resolution.
- Always use container names instead of IP addresses.
- Host ports must be unique.
- Container ports can be the same.
- Learn to troubleshoot using `docker network`, `docker inspect`, `docker logs`, and `docker exec`.

---

# Commands Practiced Today

```powershell
docker network ls
docker network inspect bridge
docker network create devops-net
docker network connect devops-net nginx-lab
docker network connect devops-net nginx-lab-2
docker network inspect devops-net
docker inspect --format='{{.NetworkSettings.Networks.bridge.IPAddress}}' nginx-lab
docker port nginx-lab
docker run -d --name nginx-lab -p 8080:80 nginx
docker run -d --name nginx-lab-2 -p 8081:80 nginx
docker run --rm --network devops-net curlimages/curl -I http://nginx-lab
docker run --rm --network devops-net curlimages/curl -I http://nginx-lab-2
docker ps
docker logs nginx-lab
docker exec -it nginx-lab bash
docker top nginx-lab
docker inspect nginx-lab
```

---

# Day 2 Status

✅ Docker Networking Completed

**Next Topic:** Docker Day 3 - Volumes & Persistent Storage
