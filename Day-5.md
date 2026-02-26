# 🐳 Docker Compose – Multi-Container Application Lab

## 📌 Lab Objective

In this lab, I practiced running multi-container applications using Docker Compose.

Instead of manually creating:
- Docker networks
- Docker volumes
- Containers one by one

Docker Compose allows defining everything inside a single `docker-compose.yml` file and running it using one command.

This lab covers:
- Single container setup
- Multi-container (WordPress + MySQL)
- Named volumes
- Default networking
- Compose commands
- Environment variables
- Persistence verification

---

# 🔹 Understanding Docker Compose

Docker Compose is a tool used to define and manage multi-container Docker applications using a YAML file.

Key Concepts:

### 1️⃣ Services
Each container is defined as a service inside `docker-compose.yml`.

### 2️⃣ Default Network
Compose automatically creates a bridge network.
All services can communicate using their service names as DNS.

Example:
If service name is `db`, other containers connect using:
db:3306

### 3️⃣ Named Volumes
Used to persist data outside containers.
Even if containers are removed, volume data remains.

---

# 🔹 Single Container Setup (Nginx)

## Step 1: Create Project Directory

[mkdir compose-basics]  
[cd compose-basics]

## Step 2: Create docker-compose.yml

[vim docker-compose.yml]

### docker-compose.yml

```yaml
services:
  nginx:
    image: nginx:latest
    container_name: nginx-compose
    ports:
      - "80:80"
```

### Explanation

- `image: nginx:latest` → Pulls official Nginx image
- `ports: "80:80"` → Maps host port 80 to container port 80
- `container_name` → Custom container name

## Step 3: Start Service

[docker compose up -d]

## Verify Running Container

[docker compose ps]

---

## 🌐 Accessing Nginx in Browser

Opened in browser:

http://192.168.31.96

---

## 📸 Screenshot Explanation

<img width="607" height="436" alt="task2-1" src="https://github.com/user-attachments/assets/320d459f-54db-4644-b230-171fe81497b2" />

<img width="1918" height="336" alt="task2-2" src="https://github.com/user-attachments/assets/233287f3-e46a-4129-a18e-b93598b65ef1" />


The screenshot shows:
- Nginx welcome/static page
- Server IP 192.168.31.96
- HTTP connection working successfully

This confirms:
- Container is running
- Port mapping is correct
- Network is working properly

---

## Stop and Remove

[docker compose down]

This removes:
- Containers
- Default network

---

# 🔹 Multi-Container Setup (WordPress + MySQL)

Now running a real multi-container application.

Architecture:

Browser → WordPress Container → MySQL Container → Named Volume

---

## docker-compose.yml

```yaml
services:
  db:
    image: mysql:8.0
    container_name: wordpress-db
    restart: always
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
      MYSQL_ROOT_PASSWORD: root
    volumes:
      - db-data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wordpress-app
    restart: always
    ports:
      - "8008:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

volumes:
  db-data:
```

---

## 🔎 Explanation

### MySQL Service (db)

- Uses official MySQL 8.0 image
- Environment variables configure database
- Named volume `db-data` stores database files
- `/var/lib/mysql` is MySQL data directory

### WordPress Service

- Uses official WordPress image
- Port mapping `8008:80`
- Connects to MySQL using:
  WORDPRESS_DB_HOST: db:3306
- `depends_on` ensures DB starts first

---

## Start Multi-Container Application

[docker compose up -d]

## Verify

[docker compose ps]

---

## 🌐 Access WordPress

http://192.168.31.96:8008

Complete WordPress installation.

---

## 📸 Screenshot Explanation

<img width="1645" height="828" alt="task3-1" src="https://github.com/user-attachments/assets/953bb9ed-e2ed-4f94-8634-a0f17ad6af71" />

<img width="1891" height="867" alt="task3-2" src="https://github.com/user-attachments/assets/d68824c2-0d18-425d-b190-5b597a7cd20d" />

<img width="1855" height="781" alt="task3--3" src="https://github.com/user-attachments/assets/433d8d81-6513-4e33-80c9-6a166e09ac37" />


The screenshot confirms:
- WordPress is accessible
- Database connection is successful
- Services are communicating via default network
- Port mapping is working correctly

---

# 🔁 Persistence Verification

Stop services:

[docker compose down]

Start again:

[docker compose up -d]

Result:
✔ WordPress configuration remained
✔ Data was not lost

Reason:
Named volume `db-data` persists MySQL data even after container removal.

---

# 🔹 Important Compose Commands Practiced

Start in detached mode:
[docker compose up -d]

View running services:
[docker compose ps]

View logs:
[docker compose logs]

Follow logs:
[docker compose logs -f]

Logs for specific service:
[docker compose logs wordpress]

Stop services without removing:
[docker compose stop]

Remove containers and networks:
[docker compose down]

Remove including volumes:
[docker compose down -v]

Rebuild after changes:
[docker compose up -d --build]

---

# 🔹 Environment Variables

## Directly Inside Compose File

```yaml
environment:
  MYSQL_DATABASE: wordpress
  MYSQL_USER: wordpress
```

## Using .env File

Create file:

[vim .env]

Add:

MYSQL_DATABASE=wordpress  
MYSQL_USER=wordpress  
MYSQL_PASSWORD=wordpress  
MYSQL_ROOT_PASSWORD=root  

Reference inside compose file:

```yaml
environment:
  MYSQL_DATABASE: ${MYSQL_DATABASE}
  MYSQL_USER: ${MYSQL_USER}
```

## Verify Variables

[docker compose config]

OR

[docker compose exec db env]

This confirms variables are properly loaded.

---

# 🧠 Key Learnings From This Lab

- Docker Compose simplifies multi-container setup.
- Service names act as DNS inside the default network.
- Named volumes ensure data persistence.
- Environment variables help manage configuration cleanly.
- One command can start a complete application stack.

---

# ✅ Conclusion

This lab demonstrated how to:

- Run single and multi-container applications
- Use default networking
- Configure environment variables
- Implement data persistence
- Manage services using Compose commands

Docker Compose significantly simplifies application deployment and management in development environments.

---
