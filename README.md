# 🐳 Docker – Theory & Hands-On Practice

Welcome to my Docker learning repository 🚀  
This repo documents my complete journey of understanding Docker — from core theory to real-world hands-on tasks.

---

# 📌 About This Repository

This repository contains:

- 📘 Detailed Docker theory notes
- 🛠 Practical task implementations
- 📂 Command explanations with outputs
- 🧪 Mini projects
- 🖥 Terminal execution results
- 📷 Output screenshots (where applicable)

The goal is to build strong containerization fundamentals and practical DevOps skills.

---

# 🐳 What is Docker?

Docker is a containerization platform that allows developers to package applications and their dependencies into lightweight, portable containers.

Containers ensure:
- Consistent environments
- Faster deployments
- Isolation of applications
- Scalability

---

# 📚 Topics Covered

## 1️⃣ Docker Basics
- What is Containerization
- Virtual Machines vs Containers
- Docker Architecture
- Docker Engine
- Docker CLI

## 2️⃣ Docker Installation & Setup
- Installing Docker
- Verifying Installation  
  `[docker --version]`
- Checking Docker Info  
  `[docker info]`

## 3️⃣ Working with Images
- Pulling images  
  `[docker pull nginx]`
- Listing images  
  `[docker images]`
- Removing images  
  `[docker rmi image_name]`

## 4️⃣ Working with Containers
- Running a container  
  `[docker run nginx]`
- Running container in detached mode  
  `[docker run -d nginx]`
- Listing running containers  
  `[docker ps]`
- Listing all containers  
  `[docker ps -a]`
- Stopping container  
  `[docker stop container_id]`
- Removing container  
  `[docker rm container_id]`

## 5️⃣ Port Mapping
- Exposing ports  
  `[docker run -d -p 8080:80 nginx]`

## 6️⃣ Docker Volumes
- Creating volume  
  `[docker volume create myvolume]`
- Listing volumes  
  `[docker volume ls]`
- Mounting volume  
  `[docker run -d -v myvolume:/data nginx]`

## 7️⃣ Docker Networks
- Listing networks  
  `[docker network ls]`
- Creating network  
  `[docker network create mynetwork]`
- Running container on custom network  
  `[docker run -d --network mynetwork nginx]`

## 8️⃣ Dockerfile
- Creating a Docker image using Dockerfile
- Building image  
  `[docker build -t myimage .]`
- Running custom image  
  `[docker run -d myimage]`

---

# 🧪 Hands-On Tasks

Each section includes:

- Task Objective
- Step-by-step commands
- Explanation of commands
- Output verification
- Screenshots (where applicable)

This ensures practical understanding instead of just theory.

---

# 🎯 Learning Outcomes

By completing this repository, I have:

- Understood Docker architecture deeply
- Built and managed containers
- Created custom Docker images
- Worked with volumes and networks
- Practiced real DevOps use cases

---

# 🚀 Why This Repository?

This repository serves as:

- 📘 My Docker documentation
- 💼 Proof of hands-on DevOps learning
- 📖 A beginner-friendly Docker reference
- 🔁 A revision guide for future use

---

# 🔮 Upcoming Additions

- Docker Compose
- Multi-container applications
- Docker best practices
- Production-ready setups
- Real-world DevOps mini projects

---

# 🤝 Connect With Me

If you're learning Docker or DevOps, feel free to connect and collaborate!

⭐ If you find this repository helpful, consider giving it a star.

---

**Author:** Devesh Patil  
**Focus:** DevOps | Cloud | Containerization | Automation  

---
