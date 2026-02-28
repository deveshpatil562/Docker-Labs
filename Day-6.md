# Docker Compose – Real World Multi-Container Application

## 📌 Project Overview

This project demonstrates how to build and run a real-world multi-container application using Docker Compose.

Instead of running single containers manually, this setup includes:

- 🐍 Python Flask Web Application
- 🗄 MySQL Database
- 🔴 Redis Cache
- 🗂 Named Volumes for persistence
- 🔁 Restart policies
- 🔗 Service-to-service communication

All required files are available inside the `day-34` folder in my GitHub repository.

---

# 🏗 Architecture

The application stack consists of 3 services:

1. **web** → Flask application  
2. **db** → MySQL 8.0 database  
3. **cache** → Redis 7  

All services communicate internally using Docker Compose networking.

Service Communication:

- Web connects to MySQL using hostname: `db`
- Web connects to Redis using hostname: `cache`
- No service uses `localhost`

---

# 📁 Project Structure

```
day-34/
│
├── docker-compose.yml
├── web/
│   ├── Dockerfile
│   ├── requirements.txt
│   ├── app.py
│   └── templates/
│       └── index.html
```

---

# 🔹 docker-compose.yml

[docker-compose.yml]

[
version: "3.9"

services:
  web:
    build: ./web
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=mysql+pymysql://root:rootpassword@db:3306/mydb
      - REDIS_URL=redis://cache:6379/0
    depends_on:
      - db
      - cache

  db:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: mydb
    volumes:
      - db_data:/var/lib/mysql

  cache:
    image: redis:7
    restart: always

volumes:
  db_data:
]

---

# 🔹 Web Application Dockerfile

[web/Dockerfile]

[
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
]

### 🔎 Explanation

- `python:3.11-slim` → lightweight Python image
- `WORKDIR /app` → sets working directory
- `COPY requirements.txt` → copy dependencies file
- `pip install` → install Flask and other packages
- `EXPOSE 5000` → application runs on port 5000
- `CMD` → runs Flask app

---

# 🔹 Flask Application Code

[web/app.py]

[
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def home():
    return render_template("index.html")

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
]

### 🔎 Explanation

- Flask app serves index.html
- Runs on `0.0.0.0` to allow container access
- Port 5000 exposed to host

---

# 🚀 Running the Stack

Start services in detached mode:

[docker compose up -d]

Check running containers:

[docker compose ps]

View logs:

[docker compose logs -f]

Stop all services:

[docker compose down]

Rebuild after code changes:

[docker compose up --build]

---

# 🔗 Service Communication Explained

Inside Docker network:

- MySQL is reachable at: `db:3306`
- Redis is reachable at: `cache:6379`

Environment variables used:

DATABASE_URL:
mysql+pymysql://root:rootpassword@db:3306/mydb

REDIS_URL:
redis://cache:6379/0

Important:
Containers must use service names, not localhost.

---

# 💾 Named Volume for Database

Defined volume:

[
volumes:
  db_data:
]

Mounted as:

[
- db_data:/var/lib/mysql
]

Purpose:

- Persist database data
- Prevent data loss when container stops
- Production-like storage behavior

---

# 🔁 Restart Policies

Used:

[
restart: always
]

Behavior:

- If container crashes → it restarts automatically
- If manually killed → it restarts
- Useful for production environments

---

# 🖼 Screenshots (Execution Proof)

## 📸 1. Docker Compose Up

(Add screenshot showing containers starting successfully)

## 📸 2. Docker Compose Logs

(Add screenshot showing MySQL, Redis, and Web logs running)

Use this to explain:

- MySQL initialized
- Redis started successfully
- Flask app serving on port 5000

## 📸 3. Browser Output

(Add screenshot showing application running at http://localhost:5000)

---

# 🧠 Key Learnings

- Multi-container architecture using Docker Compose
- Service-to-service communication
- Named volumes for persistence
- Restart policies
- Custom Dockerfile integration
- Difference between container start vs application readiness
- Production-style application structure

---

# 📌 Repository

All files are available in my GitHub repository under:

`day-34` folder

---

# ✅ Conclusion

This project demonstrates a real-world multi-container application stack using:

- Flask
- MySQL
- Redis
- Docker Compose
- Named Volumes
- Restart Policies

This setup reflects how production applications are structured in containerized environments.
