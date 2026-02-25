# Docker Volumes & Networking – Personal Lab Documentation

---

# 📚 Concept Explanation

Before jumping into hands-on tasks, it’s important to clearly understand the core problems Docker solves here.

---

## 🐳 1️⃣ The Ephemeral Nature of Containers

By default, Docker containers are **ephemeral**.

This means:

- When a container is deleted, its writable layer is deleted.
- Any data created inside the container is permanently lost.
- Containers are designed to be temporary runtime environments.

So if a database runs inside a container **without external storage**, all data disappears when the container is removed.

This is not a bug — it’s by design.

---

## 💾 2️⃣ Docker Volumes (Persistent Storage)

A Docker Volume:

- Is managed by Docker
- Lives outside the container lifecycle
- Persists even if the container is deleted
- Is stored in Docker’s internal directory (`/var/lib/docker/volumes/`)

Volumes are ideal for:

- Databases
- Stateful applications
- Production workloads

Volumes ensure data durability.

---

## 📂 3️⃣ Bind Mounts

Bind mounts directly connect:

Host Machine Folder → Container Folder

Characteristics:

- Managed by the host
- Depends on absolute host path
- Great for development
- Changes reflect instantly

Example: Editing an `index.html` file locally and seeing changes immediately in a running container.

---

## 🌐 4️⃣ Docker Networking Basics

Docker provides different network drivers:

- bridge (default)
- host
- none
- user-defined bridge (custom networks)

Important behavior:

- Default bridge → No automatic DNS resolution between containers
- Custom bridge network → Built-in DNS resolution (containers can communicate by name)

This becomes critical when running multi-container applications.

---

# 🔥 Task 1 – Data Loss Without Volume

### Run Database Container

[ docker run -d --name mydb -e POSTGRES_PASSWORD=pass postgres ]

### Create Sample Data

[ docker exec -it mydb psql -U postgres ]

Create table and insert some rows.

### Stop & Remove Container

[ docker stop mydb ]  
[ docker rm mydb ]

### Start Fresh Container Again

[ docker run -d --name mydb -e POSTGRES_PASSWORD=pass postgres ]

### ❗ Result

All previously created data was gone.

### 🧠 Explanation

Because no volume was attached:

- Data was stored inside container writable layer
- Removing container deleted that layer
- Therefore, data was permanently lost

This demonstrates why volumes are necessary for databases.

---

# 💾 Task 2 – Named Volume (Persistent Data)

### Create Named Volume

[ docker volume create my-db-volume ]

### Run Database with Volume Attached

[ docker run -d --name mydb -e POSTGRES_PASSWORD=pass -v my-db-volume:/var/lib/postgresql/data postgres ]

### Insert Data Again

[ docker exec -it mydb psql -U postgres ]

### Remove Container

[ docker stop mydb ]  
[ docker rm mydb ]

### Recreate Container Using Same Volume

[ docker run -d --name mydb -e POSTGRES_PASSWORD=pass -v my-db-volume:/var/lib/postgresql/data postgres ]

### ✅ Result

Data was still present.

### Verify Volume

[ docker volume ls ]  
[ docker volume inspect my-db-volume ]

### 🧠 Why It Worked

- Volume exists independently of container
- Container uses volume as external storage
- Removing container does NOT remove volume

This ensures persistence.

---

# 📂 Task 3 – Bind Mount (Live File Sync)

### Create Folder on Host

Create a folder and inside it create:

index.html

With content:

Hello from Bind Mount!

### Run Nginx with Bind Mount

[ docker run -d --name mynginx -p 8080:80 -v /your/host/path:/usr/share/nginx/html nginx ]

### Access in Browser

Open:

http://192.168.31.96:8080

---

## 📸 Screenshot Output

<img width="1445" height="310" alt="task3-1" src="https://github.com/user-attachments/assets/6dc3da30-7b38-4501-8b0e-d606ffc1c64f" />

<img width="1688" height="207" alt="task3-2" src="https://github.com/user-attachments/assets/41740a42-273c-41f7-9698-1bdebaf02c0f" />

<img width="467" height="172" alt="task3-3" src="https://github.com/user-attachments/assets/91842746-18e6-4e33-8850-08afb6e5e549" />

<img width="1675" height="192" alt="task3-4" src="https://github.com/user-attachments/assets/8adb7ce3-da74-42a7-80c3-dc69239165e0" />


The page displays:

Hello from Bind Mount!

---

### 🔎 What This Proves

- The container is serving files from host directory
- Any change made in host `index.html`
- Immediately reflects in browser after refresh

This confirms bind mount is directly mapping host → container.

---

## 📘 Named Volume vs Bind Mount (Clear Difference)

| Feature | Named Volume | Bind Mount |
|----------|--------------|------------|
| Managed by | Docker | Host |
| Storage Location | Docker directory | Host path |
| Portability | High | Depends on host |
| Best For | Databases | Development |
| Auto-Creation | Yes | No |

---

# 🌐 Task 4 – Default Bridge Networking

### List Networks

[ docker network ls ]

### Inspect Default Bridge

[ docker network inspect bridge ]

### Run Two Containers

[ docker run -dit --name c1 ubuntu ]  
[ docker run -dit --name c2 ubuntu ]

### Try Ping by Name

[ docker exec c1 ping c2 ]

❌ Fails

### Try Ping by IP

[ docker inspect c2 ]

[ docker exec c1 ping <container_ip> ]

✅ Works

### 🧠 Explanation

Default bridge network:

- Does not provide automatic DNS resolution
- Containers cannot resolve each other by name

---

# 🌉 Task 5 – Custom Bridge Network

### Create Custom Network

[ docker network create my-app-net ]

### Run Containers in Custom Network

[ docker run -dit --name c1 --network my-app-net ubuntu ]  
[ docker run -dit --name c2 --network my-app-net ubuntu ]

### Ping by Name

[ docker exec c1 ping c2 ]

✅ Works successfully

### 🧠 Why?

User-defined bridge networks include:

- Built-in DNS server
- Automatic name resolution
- Better isolation

---

# 🧩 Task 6 – Combined Architecture Test

### Create Custom Network

[ docker network create my-app-net ]

### Run Database with Volume on Network

[ docker run -d --name mydb --network my-app-net -e POSTGRES_PASSWORD=pass -v my-db-volume:/var/lib/postgresql/data postgres ]

### Run Application Container on Same Network

[ docker run -dit --name myapp --network my-app-net ubuntu ]

### Verify Communication

[ docker exec myapp ping mydb ]

✅ Application container successfully reached database using container name.

---

# 🧠 Final Observations

✔ Containers are ephemeral by default  
✔ Volumes solve persistence problem  
✔ Bind mounts enable real-time development  
✔ Default bridge lacks DNS resolution  
✔ Custom networks enable name-based communication  
✔ Proper networking + volumes = Production-ready setup  

---

# 🏁 Lab Conclusion

This lab demonstrated:

- Why databases must use volumes
- How bind mounts differ from volumes
- Why custom networks are essential in multi-container setups
- How Docker isolates and connects containers internally

This setup forms the foundation of real-world containerized application architecture.
