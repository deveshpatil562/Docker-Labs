# Docker Images & Container Lifecycle – Detailed Notes

This document is my complete revision guide for:
- Docker Images
- Image Layers & Caching
- Container Lifecycle
- Working with Running Containers
- Cleanup & Disk Management

This is written in detail so I can revise everything quickly in future.

---

# 🧠 1️⃣ Docker Images – Complete Understanding

## 🔹 What is a Docker Image?

A Docker Image is a **read-only template** used to create containers.

Think of it like:
- Image = Blueprint
- Container = Running object built from blueprint

Important:
- Images cannot change
- Containers can change (they get a writable layer on top)

One image can create multiple containers.

Example:
The `nginx` image can create:
- container-lifecycle
- nginx-server
- any number of other containers

---

## 🔹 Pull Required Images

I pulled official images from Docker Hub.

[ docker pull nginx ]  
[ docker pull ubuntu ]  
[ docker pull alpine ]

What happens internally:
- Docker checks local cache
- If not present → pulls layers from Docker Hub
- Stores them in local image store

---

## 🔹 List All Images

[ docker images ]

What I observed:
- ubuntu is larger (~70MB+)
- alpine is very small (~5MB)
- nginx size depends on base image

### Why Alpine is Smaller?

Alpine:
- Minimal Linux distribution
- Uses musl libc instead of glibc
- Fewer preinstalled packages
- Designed specifically for containers

Ubuntu:
- Full Linux distribution
- More libraries and utilities included

Conclusion:
Alpine is better when:
- You want lightweight images
- You want faster pull times
- You want minimal attack surface

---

## 📸 Screenshot – Images List

<img width="868" height="308" alt="task1-1" src="https://github.com/user-attachments/assets/2d5bea05-92aa-4a1a-a09f-7041e83f9743" />

<img width="1568" height="855" alt="task1-2" src="https://github.com/user-attachments/assets/2934c8d9-6e2e-46b4-b6f2-ac4beb6cc8ae" />


---

## 🔹 Inspect an Image

[ docker inspect nginx ]

This command gives detailed JSON output.

Important fields I can see:
- Image ID
- Repo tags
- Created time
- Architecture
- OS
- Environment variables
- Entrypoint
- Working directory
- RootFS layers

Why inspect is important:
- Helps debug image configuration
- Helps understand how container will behave

---

## 🔹 Remove an Image

[ docker rmi ubuntu ]

Important:
- Image cannot be removed if containers are using it.
- Must remove containers first.

---

# 🧱 2️⃣ Image Layers & Caching

## 🔹 What Are Layers?

Every Docker image is built in layers.

Each instruction in Dockerfile creates a layer:
- FROM
- RUN
- COPY
- ADD
- ENV

Layers are:
- Read-only
- Reusable
- Cached

---

## 🔹 View Image History

[ docker image history nginx ]

What I observed:
- Each row = one layer
- Some layers show size (file system change)
- Some show 0B (metadata only)

Why some layers show 0B?
Because:
- Some instructions only modify metadata
- They don’t add files

---

## 🔹 Why Docker Uses Layers?

1. Faster builds (layer caching)
2. Efficient storage
3. Faster image distribution
4. Shared base layers across images

Example:
If ubuntu base layer already exists,
Docker will not download it again.

---

## 📸 Screenshot – Image History

<img width="1246" height="431" alt="task2" src="https://github.com/user-attachments/assets/64ff6d59-e71b-4b84-a6f9-abbc3b52a987" />


---

# 🔄 3️⃣ Container Lifecycle – Full Practice

Container lifecycle states:
- Created
- Running
- Paused
- Exited
- Removed

I practiced the full lifecycle.

---

## 🔹 Create Container (Without Starting)

[ docker create --name container-lifecycle nginx ]

State:
Created

What this does:
- Creates container metadata
- Allocates writable layer
- Does NOT start the process

---

## 🔹 Start Container

[ docker start container-lifecycle ]

State:
Up / Running

This:
- Starts the main process defined in image
- For nginx → starts nginx server

---

## 🔹 Pause Container

[ docker pause container-lifecycle ]

Check status:

[ docker ps -a ]

State:
Paused

Pause means:
- Freezes container processes
- Uses Linux cgroups freezer
- Container is not stopped

---

## 🔹 Unpause Container

[ docker unpause container-lifecycle ]

State:
Running again

---

## 🔹 Stop Container

[ docker stop container-lifecycle ]

State:
Exited

Stop:
- Sends SIGTERM
- Waits for graceful shutdown
- Then SIGKILL if needed

---

## 🔹 Restart Container

[ docker restart container-lifecycle ]

This:
- Stops container
- Starts it again

---

## 🔹 Kill Container

[ docker kill container-lifecycle ]

This:
- Sends SIGKILL immediately
- Force stops container

Exit code often 137

---

## 🔹 Remove Container

[ docker rm container-lifecycle ]

Container deleted permanently.

---

## 📸 Screenshot – Lifecycle Transitions

<img width="1561" height="782" alt="task3-1" src="https://github.com/user-attachments/assets/8ec4d4b8-6e46-4bc8-b63a-5d747737f781" />

<img width="1562" height="727" alt="task3-2" src="https://github.com/user-attachments/assets/97c741b4-1c7d-4fa0-b20f-6a393a22406f" />

---

# 🌐 4️⃣ Working with Running Containers

## 🔹 Run Nginx in Detached Mode

[ docker run -d -p 80:80 --name nginx-server nginx ]

Explanation:
- -d → detached mode
- -p 80:80 → host port 80 mapped to container 80
- --name → custom name

---

## 🔹 View Logs

[ docker logs nginx-server ]

Shows stdout & stderr of container.

---

## 🔹 Follow Logs in Real-Time

[ docker logs -f nginx-server ]

Useful for:
- Debugging
- Monitoring

---

## 🔹 Exec into Container

[ docker exec -it nginx-server /bin/sh ]

Now I can explore inside container.

Commands I ran:

[ ls ]  
[ cd /usr/share/nginx/html ]  
[ pwd ]

Important:
This does not restart container.
It runs an additional process inside container.

---

## 🔹 Run Single Command Without Entering

[ docker exec nginx-server ls / ]

This runs one command and exits.

---

## 🔹 Inspect Container

[ docker inspect nginx-server ]

Important fields I checked:
- IP Address
- Network settings
- Port bindings
- Mounts
- Container state
- PID

---

## 📸 Screenshot – Inspect Output

<img width="1052" height="807" alt="task4-1" src="https://github.com/user-attachments/assets/e863f3c2-c30a-443f-bc38-77564de551c9" />

<img width="1476" height="226" alt="task-4-2" src="https://github.com/user-attachments/assets/d080f1ed-54d6-4fca-9724-3ed44acc2d4e" />

<img width="1557" height="848" alt="task4-3" src="https://github.com/user-attachments/assets/465d77ed-0b51-4cbf-b7bf-17f9f4a3b7b4" />

---

# 🧹 5️⃣ Cleanup & Disk Management

## 🔹 Stop All Running Containers

[ docker stop $(docker ps -q) ]

Stops every running container.

---

## 🔹 Remove All Stopped Containers

[ docker container prune ]

Removes:
- All stopped containers

---

## 🔹 Remove Unused Images

[ docker image prune ]

Removes:
- Dangling images

---

## 🔹 Check Docker Disk Usage

[ docker system df ]

Shows:
- Image space usage
- Container space usage
- Volume space usage
- Build cache usage

---

## 🔹 Full System Cleanup (Careful)

[ docker system prune ]

Removes:
- Stopped containers
- Unused networks
- Dangling images
- Build cache

Use carefully in production.

---

# 📸 Screenshot – Cleanup Commands

<img width="912" height="773" alt="task5" src="https://github.com/user-attachments/assets/12b0c076-b56d-46aa-be80-126ec7362b48" />


---

# 📝 Final Revision Notes

✔ Images are immutable  
✔ Containers add writable layer  
✔ Layers improve efficiency  
✔ Alpine is lightweight and minimal  
✔ Lifecycle management is essential  
✔ Cleanup prevents disk bloat  

---

# 🚀 Personal Takeaway

Understanding images + lifecycle deeply is critical before moving to:
- Dockerfiles
- Volumes
- Networks
- Docker Compose
- Kubernetes

This forms the foundation of containerization.
