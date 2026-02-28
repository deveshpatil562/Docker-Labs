# 🚀 Simple Docker Lab Task  
## Flask + MySQL + Redis Stack using Docker Compose

This is a simple multi-container Docker project added to my **Docker Lab Repository**.  
The goal of this task is to deploy a Flask application connected to MySQL and Redis using Docker Compose.

This README explains:
- Project structure
- All configuration files
- Step-by-step procedure to run
- GitHub push process

---

# 📌 Objective

Deploy a 3-service stack:

- 🐍 Flask Web App (Python 3.11)
- 🗄 MySQL Database (v8.0)
- ⚡ Redis Cache (v7)

All services are managed using Docker Compose.

---

# 📂 Project Structure

```
day-34/
├── docker-compose.yml
└── web/
    ├── Dockerfile
    ├── requirements.txt
    ├── app.py
    ├── templates/
    │   └── index.html
    └── static/
        └── code.jpg
```

---

# 📝 1️⃣ docker-compose.yml

This file defines all services and networking.

```
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
```

### Explanation

- **web** → Flask app container
- **db** → MySQL container with persistent volume
- **cache** → Redis container
- `depends_on` ensures DB and Redis start before web
- `db_data` volume stores MySQL data persistently

---

# 📝 2️⃣ Dockerfile (Inside web/)

```
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

### Explanation

- Uses lightweight Python image
- Sets working directory
- Installs dependencies
- Exposes port 5000
- Runs Flask app

---

# 📝 3️⃣ requirements.txt

```
flask
pymysql
sqlalchemy
redis
```

These libraries allow:
- Flask → Web framework
- PyMySQL → MySQL connectivity
- SQLAlchemy → ORM
- Redis → Cache connection

---

# 📝 4️⃣ app.py

```
from flask import Flask, render_template
import redis, pymysql

app = Flask(__name__)

@app.route("/")
def home():
    return render_template("index.html")

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

This is a basic Flask application serving an HTML page.

---

# 📝 5️⃣ index.html (templates/)

```
<!DOCTYPE html>
<html>
<head>
    <title>Day-34 Flask App</title>
</head>
<body style="background: url('{{ url_for('static', filename='code.jpg') }}') no-repeat center center fixed; background-size: cover;">
    <h1>✨ Welcome to My Flask App ✨</h1>
    <p>DevOps is a journey — enjoy it!</p>
</body>
</html>
```

---

# ▶️ How to Run This Project

### Step 1: Navigate to Project Directory

```
cd day-34
```

### Step 2: Build and Start Containers

```
docker compose up --build
```

### Step 3: Verify Running Containers

```
docker ps
```

### Step 4: Access Application

Open in browser:

```
http://localhost:5000
```

You should see the Flask application running.

---

# 🛑 Stop the Stack

```
docker compose down
```

To remove volumes also:

```
docker compose down -v
```

---

# 🔍 Useful Debug Commands

View logs:

```
docker compose logs
```

Follow logs:

```
docker compose logs -f
```

Enter web container:

```
docker compose exec web sh
```

---

# 🚀 Push to GitHub (Procedure Followed)

Clone repository:

```
git clone https://github.com/deveshpatil562/Docker-Labs.git
```

Copy project folder into repo:

```
cp -r day-34 Docker-Labs/
```

Remove nested git folder if exists:

```
rm -rf Docker-Labs/day-34/.git
```

Commit and push:

```
cd Docker-Labs
git add day-34
git commit -m "Add Flask + MySQL + Redis Docker Compose Lab"
git push origin main
```

---

# 🔐 GitHub Authentication Note

GitHub no longer supports password authentication.

Use:
- Personal Access Token (PAT), or
- SSH keys (Recommended)

---

# ✅ Final Outcome

✔ Multi-container application deployed  
✔ Services connected internally via Docker network  
✔ Persistent MySQL storage using volumes  
✔ Application accessible via browser  
✔ Project pushed to GitHub  

---

# 🏁 Conclusion

This lab demonstrates:

- Dockerfile creation  
- Multi-container orchestration  
- Service networking  
- Volume management  
- Real-world application deployment  

This is a simple but practical Docker Compose project added to my Docker Lab repository for future revision and practice.

---

Happy Learning 🚀
