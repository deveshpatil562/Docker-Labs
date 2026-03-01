# 🐳 Docker Multi-Stage Build Lab – Detailed Guide + Troubleshooting

This document is part of my **personal Docker lab repository**.  
In this lab, I worked on:

- Understanding large vs optimized Docker images  
- Implementing multi-stage builds  
- Applying image best practices  
- Pushing images to Docker Hub  
- Debugging real-world Docker & Go build issues  

Below is a **detailed explanation of each concept**, followed by tasks, screenshots placement, and finally a full troubleshooting log with errors and fixes.

---

# 📘 Understanding the Core Concepts

---

## 1️⃣ Why Large Docker Images Are a Problem

When we use full base images like:

[FROM golang:1.25.5]

Docker includes:

- Full OS layer
- Build tools
- Package managers
- Caches
- Compilers
- Debug utilities

This increases:

- Image size
- Deployment time
- Security attack surface
- Storage consumption
- CI/CD build time

Large images are inefficient for production.

---

## 2️⃣ What is a Multi-Stage Build?

Multi-stage builds allow us to:

- Build the application in one stage (builder stage)
- Copy only the compiled binary into a minimal runtime image
- Discard unnecessary build dependencies

Example structure:

[FROM golang:1.25.5 AS builder]  
[FROM alpine:3.19]

This dramatically reduces image size.

---

## 3️⃣ Why Alpine is Smaller

Alpine Linux:

- Minimal base OS (~5MB)
- Fewer packages
- Designed for containers

Compared to Ubuntu or full Go images, Alpine drastically reduces final size.

---

## 4️⃣ Why Not Run Containers as Root?

By default, containers run as root.

Security risk:

- If compromised → attacker gets root access inside container
- Possible privilege escalation

Best practice:

[RUN adduser -D appuser]  
[USER appuser]

---

## 5️⃣ Why Use Specific Tags Instead of latest?

Using:

[FROM golang:latest]

Can break builds when new versions are released.

Better:

[FROM golang:1.25.5]

Ensures:

- Reproducible builds
- Stability
- Predictable behavior

---

# 🚀 Task Implementation

---

# ✅ Task 1 – Single Stage Go API

## Objective

- Build Go API using single stage
- Check image size
- Observe large size

---

## Single Stage Dockerfile

```dockerfile
FROM golang:1.25.5

WORKDIR /app

COPY go.mod .
COPY main.go .

RUN go build -o app

EXPOSE 8080

CMD ["./app"]
```

---

## Build Command

[ docker build -t go-api-single . ]

---

## Check Image Size

[ docker images go-api-single ]

---

## 📸 Screenshot Section – Task 1

<img width="1648" height="273" alt="task1" src="https://github.com/user-attachments/assets/a5bd5945-dccc-483e-9b94-b18d7570c5a2" />

<img width="1097" height="248" alt="Screenshot 2026-03-01 183414" src="https://github.com/user-attachments/assets/1b81ae45-3d90-4e13-baab-774d5fdccf1c" />


- Docker build output
- docker images output showing image size

Explain in your repo:

- Notice how large the image size is
- This includes compiler + build tools

---

# ✅ Task 2 – Multi-Stage Build

---

## Multi-Stage Dockerfile

```dockerfile
# Builder Stage
FROM golang:1.25.5 AS builder

WORKDIR /app
COPY go.mod .
COPY main.go .

RUN go build -o app

# Runtime Stage
FROM alpine:3.19

WORKDIR /app

COPY --from=builder /app/app .

EXPOSE 8080

CMD ["./app"]
```

---

## Build Multi-Stage Image

[ docker build -t go-api-multi . ]

---

## Compare Sizes

[ docker images ]

---

## 📸 Screenshot Section – Task 2

<img width="1633" height="241" alt="task2" src="https://github.com/user-attachments/assets/de3f7772-efae-43df-95ab-5b205dfe315c" />


- Size of go-api-single
- Size of go-api-multi

Explain:

- Multi-stage removed compiler
- Only binary copied
- Alpine base is minimal
- Final size significantly reduced

---

# ✅ Task 3 – Push to Docker Hub

---

## Login

[ docker login ]

---

## Tag Image

[ docker tag go-api-multi yourusername/go-api:1.0 ]

---

## Push

[ docker push yourusername/go-api:1.0 ]

---

## Verify by Pulling Again

[ docker rmi yourusername/go-api:1.0 ]  
[ docker pull yourusername/go-api:1.0 ]

---

## 📸 Screenshot Section – Task 3

<img width="1276" height="332" alt="task3-1" src="https://github.com/user-attachments/assets/507d7db6-5ada-4f2f-b0d9-825434ddbc9a" />

<img width="1645" height="397" alt="task3-2" src="https://github.com/user-attachments/assets/45156278-8fcf-416d-9a62-91ee462efb77" />


- Successful push
- Docker Hub repository page
- Tags tab
- Successful pull

Explain:

- Version tagging
- Difference between :latest and :1.0

---

# ✅ Task 4 – Apply Best Practices

Updated Dockerfile with security improvements:

```dockerfile
FROM golang:1.25.5 AS builder

WORKDIR /app
COPY go.mod .
COPY main.go .

RUN go build -o app

FROM alpine:3.19

WORKDIR /app

RUN adduser -D appuser

COPY --from=builder /app/app .

USER appuser

EXPOSE 8080

CMD ["./app"]
```

---

## Improvements Applied

✔ Multi-stage build  
✔ Minimal base image  
✔ Non-root user  
✔ Specific version tags  
✔ Reduced layers  

---

## 📸 Screenshot Section – Best Practices

<img width="1912" height="827" alt="Screenshot 2026-03-01 191018" src="https://github.com/user-attachments/assets/860427fb-e000-4ccc-8ef5-7c44585fdbcb" />


- docker images output
- Reduced size
- Container running successfully

---

# 🛠 Troubleshooting Log

This section contains all errors encountered and how I resolved them.

---

| # | Error | Cause | Solution |
|---|-------|-------|----------|
| 1 | go.mod requires go >= 1.25.5 | Dockerfile used older Go version | [FROM golang:1.25.5 AS builder] |
| 2 | Container exited immediately (Exit 2) | Ran container in foreground & pressed Ctrl+C | [docker run -d -p 8080:8080 go-api-single] |
| 3 | No space left on device | Docker build cache filled disk | [docker builder prune -f] <br> [docker system prune -af --volumes] <br> [go clean -cache -modcache -i -r] |
| 4 | docker logs showed nothing | App exited before printing logs | Added logging inside main.go |
| 5 | FROM goland typo | Spelling mistake in Dockerfile | [FROM golang:1.25.5 AS builder] |
| 6 | TLS handshake timeout pulling alpine | Network/DNS issue during image pull | Retried pull, checked internet, restarted Docker |
| 7 | docker images <container_id> showed nothing | Used container ID instead of image name | [docker images go-api-single] <br> [docker inspect go-api-single --format='{{.Size}}'] |

---

## Logging Fix Applied in main.go

```go
log.Println("Starting server...")
log.Fatalf("Server failed: %v", err)
```

This helped identify startup failures clearly.

---

# 🧠 Final Learnings

- Multi-stage builds drastically reduce image size
- Always match Go version with go.mod
- Use specific base image versions
- Clean Docker cache if disk space issues occur
- Always add logging for debugging
- Avoid running containers as root
- Understand difference between image and container

---

# 🏁 Conclusion

In this lab I:

✔ Built single-stage Go image  
✔ Converted to multi-stage build  
✔ Reduced image size significantly  
✔ Pushed image to Docker Hub  
✔ Applied production best practices  
✔ Debugged real-world Docker build errors  

This lab improved my understanding of:

- Docker optimization
- Production-ready Dockerfiles
- Troubleshooting Docker & Go build issues
- Secure container practices

---

📌 End of Multi-Stage Build + Docker Hub + Troubleshooting Lab
