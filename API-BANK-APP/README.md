# 🏦 Bank Application – Dockerized Spring Boot + MySQL

## 📌 Project Overview

This project is a **Dockerized Spring Boot Bank Application** connected to a **MySQL 8.0 database** using Docker Compose.

The goal of this project was to:

- Containerize a Java Spring Boot application
- Connect it with a MySQL database
- Use Docker Compose for multi-container orchestration
- Implement healthchecks and networking
- Use volumes for database persistence
- Follow best practices (non-root user, small image, .dockerignore, etc.)
- Push the image to Docker Hub
- Test the entire setup from scratch

This project demonstrates real-world DevOps practices for packaging and shipping applications using Docker.

---

# 🏗️ Tech Stack

- Java (Spring Boot)
- Maven
- MySQL 8.0
- Docker
- Docker Compose

---

# 📂 Project Structure

![Uploading Screenshot 2026-03-02 171852.png…]()

---

# ⚙️ How This Project Works

1. Spring Boot application runs inside a container.
2. MySQL database runs inside another container.
3. Docker Compose connects both containers using custom network `banknet`.
4. Database data is stored in a persistent volume `db_data`.
5. Healthcheck ensures MySQL is ready before application starts.
6. Application connects using environment variables.

Architecture Flow:

User → Browser → Spring Boot Container → MySQL Container → Docker Volume

---

# 🐳 Dockerfile Overview

The Dockerfile uses a multi-stage build:

Stage 1:
- Uses Maven image
- Builds the JAR file

Stage 2:
- Uses lightweight JDK slim image
- Creates non-root user
- Copies only JAR file
- Runs application securely

Build the image:

[ docker build -t bankapp:latest . ]

Verify image:

[ docker images ]

---

# 🐙 Docker Compose Configuration

This project uses version 3.9 of Docker Compose.

Services included:

1) app service
2) db service

Features implemented:

- Custom network: banknet
- Named volume: db_data
- Healthcheck for MySQL
- depends_on with condition: service_healthy
- Environment variables for DB configuration

Start full stack:

[ docker compose up -d ]

Stop services:

[ docker compose down ]

---

# 🗄️ docker-compose.yml (Reference)

version: "3.9"

services:
  app:
    build: .
    container_name: bankapp
    ports:
      - "8080:8080"
    environment:
      - SPRING_DATASOURCE_URL=jdbc:mysql://db:3306/bankdb
      - SPRING_DATASOURCE_USERNAME=bankuser
      - SPRING_DATASOURCE_PASSWORD=bankpass
    depends_on:
      db:
        condition: service_healthy
    networks:
      - banknet

  db:
    image: mysql:8.0
    container_name: bankdb
    environment:
      - MYSQL_DATABASE=bankdb
      - MYSQL_USER=bankuser
      - MYSQL_PASSWORD=bankpass
      - MYSQL_ROOT_PASSWORD=rootpass
    volumes:
      - db_data:/var/lib/mysql
    healthcheck:
      test: ["CMD-SHELL", "mysqladmin ping -h localhost -u bankuser --password=bankpass"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - banknet

volumes:
  db_data:

networks:
  banknet:

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

---

# 🚀 Step-by-Step Execution Procedure

Step 1: Build Docker Image

[ docker build -t bankapp:latest . ]

Step 2: Start Complete Application Stack

[ docker compose up -d ]

Step 3: Verify Running Containers

[ docker ps ]

Check logs if needed:

[ docker logs bankapp ]
[ docker logs bankdb ]

Step 4: Access Application

Open browser and go to:

http://localhost:8080

---

# 💾 Database Persistence

Volume used: db_data

Check volumes:

[ docker volume ls ]

Data remains safe even if containers are removed.

---

# 🐳 Push to Docker Hub

Tag image:

[ docker tag bankapp:latest <your-dockerhub-username>/bankapp:latest ]

Login:

[ docker login ]

Push image:

[ docker push <your-dockerhub-username>/bankapp:latest ]

Docker Hub Link:
(Add your Docker Hub repository link here)

---

# 🧪 Full Fresh Deployment Test

To verify production readiness:

Stop and remove everything:

[ docker compose down -v ]
[ docker rmi bankapp:latest ]

Pull from Docker Hub:

[ docker pull <your-dockerhub-username>/bankapp:latest ]

Run again:

[ docker compose up -d ]

If application works without rebuilding → Setup is correct.

---

# 🧩 Challenges Faced

1) Database started before application
   Solution: Added healthcheck + depends_on condition

2) Large image size
   Solution: Used multi-stage Docker build

3) Container communication issue
   Solution: Used custom Docker network

4) Data loss after restart
   Solution: Implemented named volume

---

# 📦 Final Image Details

Application Image: bankapp:latest  
Database Image: mysql:8.0  
Network: banknet  
Volume: db_data  
Final Image Size: (Add your actual image size)

---

# 📸 Screenshots

(Add your screenshots here)

- Docker build output
- Docker compose up output
- docker ps result
- Application running in browser
- Docker Hub repository page

---

# 🎯 What This Project Demonstrates

- Multi-container application setup
- Dockerfile optimization
- Secure container practices
- Service orchestration with Docker Compose
- Healthchecks implementation
- Persistent storage management
- Clean deployment testing
- Image publishing workflow

---

# 👨‍💻 Author

Devesh  
DevOps 90 Days Journey 🚀  
GitHub: (Add your GitHub link)  
Docker Hub: (Add your Docker Hub link)
