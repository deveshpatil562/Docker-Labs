# 🐳 Docker Lab – API Bank App (Spring Boot + MySQL)

---

# 📌 Project Overview

In this Docker Lab, I containerized my **API Bank App** (Spring Boot application) and deployed it using Docker and Docker Compose.

The complete project source code is available inside my GitHub repository under the folder:

📁 api-bank-app

This lab demonstrates how to:

- Containerize a backend API application
- Connect it to a MySQL database
- Use multi-stage Docker builds
- Implement secure container practices
- Orchestrate multi-container setup using Docker Compose
- Push the image to Docker Hub
- Test the deployment from scratch

This simulates a real-world production deployment workflow.

---

# 🏗️ Task 1: Pick Your App

I selected my own project:

📁 api-bank-app (Spring Boot API)

Why this app?

- It is a backend REST API
- It requires a database (MySQL)
- It represents a real-world microservice
- It is ideal for multi-container deployment practice

Tech Stack:

- Java (Spring Boot)
- Maven
- MySQL 8.0
- Docker
- Docker Compose

---

# 🐳 Task 2: Write the Dockerfile

## 🎯 Objective

- Create optimized Dockerfile
- Use multi-stage build
- Use non-root user
- Keep image lightweight (slim/alpine)
- Add .dockerignore
- Build and test locally

---

## 🔹 Multi-Stage Build Concept

Multi-stage builds reduce final image size.

Stage 1 (Build Stage):
- Uses Maven image
- Builds the application
- Generates JAR file

Stage 2 (Runtime Stage):
- Uses lightweight JDK image
- Copies only final JAR
- Creates non-root user
- Runs securely

This removes build dependencies from the final image.

---

## 🔹 Build the Docker Image

Navigate inside api-bank-app folder:

[ cd api-bank-app ]

Build image:

[ docker build -t bankapp:latest . ]

Verify image:

[ docker images ]

If build completes successfully, the application is now containerized.

---

# 📦 .dockerignore

The .dockerignore file prevents unnecessary files from being copied into the image.

Common ignored items:

- target/
- .git/
- logs/
- IDE files

Benefits:

- Smaller image size
- Faster build time
- Cleaner production image

---

# 🐙 Task 3: Add Docker Compose

## 🎯 Objective

Create docker-compose.yml with:

- App service (built from Dockerfile)
- MySQL database service
- Named volume for persistence
- Custom Docker network
- Environment variables
- Healthcheck for database

---

# ⚙️ Docker Compose Setup Explanation

## 1️⃣ App Service

- Built from local Dockerfile
- Exposes port 8080
- Connects to MySQL using environment variables
- Waits for DB healthcheck
- Connected to custom network

## 2️⃣ Database Service

- Uses mysql:8.0 image
- Creates database bankdb
- Creates user bankuser
- Uses named volume db_data
- Healthcheck verifies MySQL readiness

---

# 🚀 Start Full Application Stack

From api-bank-app directory:

[ docker compose up -d ]

What happens:

- MySQL container starts
- Volume db_data created
- Custom network created
- Healthcheck runs
- Spring Boot container starts after DB is healthy

---

# 🔍 Verify Running Containers

[ docker ps ]

You should see:

- bankapp container running
- bankdb container running
- Port 8080 mapped
- DB service healthy

---

# 🌐 Access the API

Open browser:

http://localhost:8080

Or test API endpoint:

[ curl http://localhost:8080 ]

If response is successful → containers are communicating correctly.

---

# 💾 Database Persistence

Named volume used:

db_data

Check volumes:

[ docker volume ls ]

Purpose:

- Data survives container restart
- Database is persistent
- Production-like behavior

---

# 🔐 Environment Variables Used

Application:

- SPRING_DATASOURCE_URL=jdbc:mysql://db:3306/bankdb
- SPRING_DATASOURCE_USERNAME=bankuser
- SPRING_DATASOURCE_PASSWORD=bankpass

Database:

- MYSQL_DATABASE=bankdb
- MYSQL_USER=bankuser
- MYSQL_PASSWORD=bankpass
- MYSQL_ROOT_PASSWORD=rootpass

This ensures configuration is externalized (best practice).

---

# 🐳 Task 4: Ship It (Push to Docker Hub)

## Tag the Image

[ docker tag bankapp:latest <your-dockerhub-username>/bankapp:latest ]

## Login to Docker Hub

[ docker login ]

## Push Image

[ docker push <your-dockerhub-username>/bankapp:latest ]

Docker Hub Repository:
(Add your Docker Hub link here)

Now the image is publicly available.

---

# 🧪 Task 5: Test the Whole Flow (Fresh Deployment Test)

This ensures the image works independently of your local build.

## Remove Everything

[ docker compose down -v ]

Remove local image:

[ docker rmi bankapp:latest ]

## Pull From Docker Hub

[ docker pull <your-dockerhub-username>/bankapp:latest ]

## Run Again

[ docker compose up -d ]

If application runs successfully → deployment is fully reproducible.

---

# 📄 Documentation File

Created additional documentation file:

day-36-docker-project.md

It includes:

- Why I chose api-bank-app
- Dockerfile explanation (line by line)
- Challenges faced
- Final image size
- Docker Hub link

---

# 🧩 Challenges Faced & Solutions

1️⃣ Application starting before DB ready  
✔ Fixed using healthcheck + depends_on condition

2️⃣ Large image size  
✔ Fixed using multi-stage build

3️⃣ Container communication issue  
✔ Solved using custom network

4️⃣ Data loss after restart  
✔ Implemented named volume

---

# 📦 Final Deployment Details

Application Image: bankapp:latest  
Database Image: mysql:8.0  
Network: banknet  
Volume: db_data  
Final Image Size: (Add your actual size here)  

---

# 🎯 What This Lab Demonstrates

- Containerizing backend APIs
- Multi-stage Docker builds
- Secure container practices
- Docker networking
- Volume persistence
- Healthchecks
- Docker Compose orchestration
- Docker Hub publishing
- Clean environment testing

This project reflects a real production-style container deployment workflow.

---

# 👨‍💻 Author

Devesh Patil
Personal Docker Lab  
GitHub Repository: [(Add your repo link)  ](https://github.com/deveshpatil562/Docker-Labs)
Folder: api-bank-app  
