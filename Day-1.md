# 🐳 Docker – Complete Theory & Hands-On Guide

---

# 📌 Introduction to Docker

## 🎯 Goal
Understand what Docker is and run your first container successfully.

---

# 🚀 What You Will Learn

- Why containers exist and how they differ from Virtual Machines
- How to install Docker
- How to run and explore containers from Docker Hub
- How to manage running and stopped containers

---

# 📦 What is Docker?

Docker is a **containerization platform** that allows developers to package applications with all their dependencies into lightweight, portable containers.

Containers:
- Run anywhere
- Start fast
- Use fewer resources than VMs
- Ensure consistency across environments

---

# 🧠 Why Do We Need Containers?

Before containers:
- "It works on my machine" problem
- Dependency conflicts
- Difficult deployments

With containers:
- Same app runs everywhere
- Easy scaling
- Faster CI/CD pipelines
- Microservices architecture support

---

# 🆚 Containers vs Virtual Machines

| Containers | Virtual Machines |
|------------|------------------|
| Lightweight | Heavy |
| Share host OS kernel | Full guest OS |
| Start in seconds | Take minutes |
| Low resource usage | High resource usage |

Containers = Process-level isolation  
VMs = Hardware-level virtualization  

---

# 🏗 Docker Architecture


![docker-architecture](https://github.com/user-attachments/assets/fd93ffbb-f767-4472-850c-80772cc37216)



Docker works using:

- **Docker Client** → Sends commands
- **Docker Daemon** → Executes commands
- **Docker Images** → Blueprints
- **Docker Containers** → Running instances of images
- **Docker Registry (Docker Hub)** → Stores images

### 🔄 Flow:
1. You run a command
2. Client talks to daemon
3. Daemon pulls image (if not present)
4. Daemon creates container
5. Container runs your application

---

# 🛠 Install Docker

Install Docker on your machine or cloud instance.

Verify installation:

[ docker --version ]

---

# 🧪 Run Hello World Container

[ docker run hello-world ]

### What Happened Internally?

1. Docker client contacted Docker daemon  
2. Docker daemon pulled image from Docker Hub  
3. Docker created a container  
4. Container executed and printed output  

---

# 🌐 Run Real Containers

## 1️⃣ Run Nginx Container

[ docker run -d -p 80:80 --name mynginx nginx ]

Now open browser:
```
http://localhost
```

---

## 2️⃣ Run Ubuntu in Interactive Mode

[ docker run -it ubuntu bash ]

You are now inside a mini Linux machine.

Try:

[ ls ]  
[ pwd ]  
[ apt update ]  

Exit using:

[ exit ]

---

# 📋 List Containers

List running containers:

[ docker ps ]

List all containers (including stopped):

[ docker ps -a ]

---

# 🛑 Stop and Remove Container

Stop container:

[ docker stop container_name ]

Remove container:

[ docker rm container_name ]


---

# 🔎 Explore More Docker Options

## Run in Detached Mode

[ docker run -d nginx ]

Detached mode runs container in background.

---

## Custom Container Name

[ docker run -d --name webserver nginx ]

---

## Port Mapping

[ docker run -d -p 8080:80 nginx ]

Format:
[ -p host_port:container_port ]

---

## Check Logs

[ docker logs container_name ]

---

## Execute Command Inside Running Container

[ docker exec -it container_name bash ]

If bash doesn't work:

[ docker exec -it container_name sh ]

---

# 🖼️ My Output Screenshots

## Hello World Output

<img width="1392" height="736" alt="dhelloworld" src="https://github.com/user-attachments/assets/774653db-5843-4626-b5fb-97e57f3527bd" />

## Running Containers Output

<img width="1546" height="688" alt="task3terminal" src="https://github.com/user-attachments/assets/43fdb809-eb7c-4d22-a5f6-701c0298e340" />

<img width="1918" height="515" alt="dnginxrun" src="https://github.com/user-attachments/assets/22c3eb42-070c-437d-971a-db6b5df8af78" />

<img width="1370" height="505" alt="dtask4" src="https://github.com/user-attachments/assets/9f1cb22a-4f3d-4531-a114-91c77198e9e3" />


---

# 📊 Sample Output From My System

Example:

```
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                 NAMES
81f4ae3ccbee   nginx     "/docker-entrypoint…"   4 minutes ago    Up 4 minutes    0.0.0.0:80->80/tcp                    friendly_jang
9ba2d07ef818   ubuntu    "/bin/bash"              14 minutes ago   Up 14 minutes                                          magical_easley
```

Hello World Message:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

---

# 💡 Important Docker Commands (Quick Reference)

[ docker run ]  
[ docker ps ]  
[ docker ps -a ]  
[ docker stop ]  
[ docker rm ]  
[ docker logs ]  
[ docker exec ]  

Flags:

[ -it ] → Interactive mode  
[ -d ] → Detached mode  
[ -p ] → Port mapping  
[ --name ] → Custom container name  

---

# 🔥 Why This Matters for DevOps

Docker is the foundation of:

- CI/CD pipelines
- Kubernetes
- Microservices
- Cloud deployments
- Scalable systems

Every modern DevOps workflow uses containers.

Learning Docker is the first step toward mastering:
- Kubernetes
- DevOps Engineering
- Cloud Native Development

---

# 🏁 Conclusion

Docker simplifies development, deployment, and scaling.

You can now:
- Run containers
- Manage containers
- Explore images
- Understand Docker architecture

This is the beginning of your container journey 🚀

---

🐳 Happy Dockering!
