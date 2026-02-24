# 🐳 Dockerfile Deep Dive – Build Your Own Custom Images

## 📌 Objective

This guide documents my hands-on practice with **Dockerfiles** — building custom images from scratch, understanding instructions, optimizing builds, and learning production-level best practices.

This is not just about running containers.  
This is about **building images like a DevOps engineer.**

---

# 📦 Task 1 – Your First Dockerfile

## 🎯 Goal
- Use Ubuntu as base
- Install curl
- Print a custom message
- Build and run image

---

## 📁 Step 1: Create Project Directory

[mkdir my-first-image]  
[cd my-first-image]

---

## 📝 Step 2: Create Dockerfile

```Dockerfile
FROM ubuntu:latest

RUN apt-get update && apt-get install -y curl

CMD ["echo", "Hello from my custom image!"]
```

---

## 🔍 Explanation

### 🔹 FROM ubuntu:latest
Defines the base image.  
Every Docker image starts from a base layer.

### 🔹 RUN apt-get update && apt-get install -y curl
Executes commands during build time.  
Creates a new image layer.

### 🔹 CMD
Default command executed when container starts.

---

## 🏗 Build Image

[docker build -t my-ubuntu:v1 .]

📌 The `.` represents the **build context** (current directory).

---

## ▶ Run Container

[docker run my-ubuntu:v1]

✅ Expected Output:
```
Hello from my custom image!
```

---

## 📸 Screenshot

<img width="1577" height="801" alt="task1" src="https://github.com/user-attachments/assets/68a66b61-5518-4430-ad15-d491de1aa2d2" />


---

# 📘 Task 2 – Understanding Dockerfile Instructions

## 🎯 Goal
Use all core Dockerfile instructions.

---

## 📝 Dockerfile Example

```Dockerfile
FROM ubuntu:latest

WORKDIR /app

RUN apt-get update && apt-get install -y curl

COPY . .

EXPOSE 8080

CMD ["bash"]
```

---

## 🧠 Instruction Breakdown

### 🔹 FROM
Base image selection.

### 🔹 WORKDIR /app
Sets working directory inside container.  
Equivalent to `cd /app`.

### 🔹 RUN
Executes build-time commands.

### 🔹 COPY . .
Copies everything from host → container.

### 🔹 EXPOSE 8080
Documents container port (does NOT publish it).

### 🔹 CMD
Default runtime command.

---

## 🏗 Build & Run

[docker build -t dockerfile-demo:v1 .]  
[docker run -it dockerfile-demo:v1]

---

## 📸 Screenshot



---

# ⚖ Task 3 – CMD vs ENTRYPOINT

Understanding this difference is critical in real-world container design.

---

## 🧪 Example 1 – CMD

```Dockerfile
FROM ubuntu
CMD ["echo", "hello"]
```

Build:

[docker build -t cmd-demo .]

Run:

[docker run cmd-demo]

Override command:

[docker run cmd-demo ls]

### 🔍 Observation

CMD gets replaced when new command is passed.

---

## 🧪 Example 2 – ENTRYPOINT

```Dockerfile
FROM ubuntu
ENTRYPOINT ["echo"]
```

Build:

[docker build -t entry-demo .]

Run:

[docker run entry-demo hello world]

### 🔍 Observation

ENTRYPOINT does NOT get replaced.  
Arguments get appended.

---

## 🧠 When to Use?

| Use Case | Instruction |
|----------|------------|
| Flexible default command | CMD |
| Container behaves like CLI tool | ENTRYPOINT |
| Want fixed executable + variable args | ENTRYPOINT + CMD |

---

## 📸 Screenshot

```
images/task3-cmd-vs-entrypoint.png
```

---

# 🌐 Task 4 – Build a Simple Web App Image

## 🎯 Goal
Serve static website using Nginx container.

---

## 📝 Step 1: Create index.html

```html
<!DOCTYPE html>
<html>
<head>
  <title>Docker Website</title>
</head>
<body>
  <h1>Hello from My Custom Nginx Image 🚀</h1>
</body>
</html>
```

---

## 📝 Step 2: Create Dockerfile

```Dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html
```

📌 Nginx serves files from:

`/usr/share/nginx/html/`

---

## 🏗 Build Image

[docker build -t my-website:v1 .]

---

## ▶ Run Container

[docker run -d -p 8080:80 my-website:v1]

Open in browser:

http://localhost:8080

---

## 📸 Screenshot

```
images/task4-nginx-output.png
```

---

# 🚫 Task 5 – .dockerignore

## 🎯 Why Important?

When building images, Docker sends everything in the build context.

Unnecessary files:
- Slow down build
- Increase image size
- May leak secrets

---

## 📝 Create .dockerignore

```
node_modules
.git
*.md
.env
```

---

## 🏗 Rebuild Image

[docker build -t optimized-image:v1 .]

Verify ignored files are not included.

---

## 📸 Screenshot

```
images/task5-dockerignore.png
```

---

# ⚡ Task 6 – Build Optimization & Layer Caching

Docker builds images in layers.

Each instruction = one layer.

If a layer changes → all layers below rebuild.

---

## 🏗 Initial Build

[docker build -t cache-demo:v1 .]

---

## ✏ Modify One Line and Rebuild

[docker build -t cache-demo:v2 .]

You’ll see:

```
CACHED
```

Docker reuses unchanged layers.

---

## 🧠 Why Layer Order Matters?

Bad Example:

```Dockerfile
COPY . .
RUN apt-get update && apt-get install -y curl
```

If code changes → dependencies reinstall.

Better Example:

```Dockerfile
RUN apt-get update && apt-get install -y curl
COPY . .
```

Now dependencies stay cached unless base changes.

---

## 📸 Screenshot (Cache Demo)

Add your highlighted build output screenshot:

```
images/task6-cache-demo.png
```

---

# 📂 Project Structure

```
my-docker-repo/
│
├── my-first-image/
│   ├── Dockerfile
│
├── nginx-app/
│   ├── Dockerfile
│   ├── index.html
│
├── images/
│   ├── task1-build-output.png
│   ├── task2-instructions-demo.png
│   ├── task3-cmd-vs-entrypoint.png
│   ├── task4-nginx-output.png
│   ├── task5-dockerignore.png
│   └── task6-cache-demo.png
│
└── dockerfile-notes.md
```

---

# 🧠 Key Takeaways

✔ Dockerfile builds custom images  
✔ Each instruction creates a layer  
✔ Layer order impacts performance  
✔ CMD vs ENTRYPOINT matters in production  
✔ .dockerignore improves speed and security  
✔ Caching dramatically reduces rebuild time  

---

# 🚀 Final Thoughts

Writing Dockerfiles properly is a **core DevOps skill**.

Running containers is easy.  
Designing efficient, production-ready images is engineering.

---

🔥 Happy Learning  
🐳 Keep Shipping with Docker
