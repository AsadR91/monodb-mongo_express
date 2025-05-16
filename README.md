## 🗃️ MongoDB & Mongo Express – Docker Setup

This guide provides a quick and easy way to run **MongoDB** and **Mongo Express** using Docker. It includes their purpose, Docker usage, and troubleshooting steps for container management.

---

## 📌 Overview

### 🔹 MongoDB

**MongoDB** is a popular **NoSQL database** that stores data in flexible, JSON-like documents. It’s ideal for scalable, high-performance applications where schema flexibility is important.

### 🔹 Mongo Express

**Mongo Express** is a lightweight, **web-based admin interface** for MongoDB. It allows you to:

* View and manage databases
* Browse collections and documents
* Execute queries and CRUD operations
* Use a user-friendly web UI to interact with MongoDB

---

## 🚀 Getting Started

Ensure Docker is installed and a custom Docker network exists:

```
docker network create project-network
```

---

## 🐳 Start MongoDB Container

```
docker run -d --name mongodb \
  --network project-network \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  -v mongo-data:/data/db \
  mongo:4.4
```

**Explanation:**

* Uses `mongo:4.4` (recommended for CPUs without AVX support)
* Stores data in a named volume: `mongo-data`
* Runs on a shared Docker network for Mongo Express to connect

---

## 🐳 Start Mongo Express Container

```
docker run -d --name mongo-express \
  --network project-network \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
  -e ME_CONFIG_MONGODB_SERVER=mongodb \
  -e ME_CONFIG_BASICAUTH_USERNAME=admin \
  -e ME_CONFIG_BASICAUTH_PASSWORD=pass123 \
  -p 8083:8081 \
  mongo-express:latest
```

**Access the Web UI:** [http://localhost:8083](http://localhost:8083)

**Login Credentials:**

* Username: `admin`
* Password: `pass123`

---

## 🛑 Troubleshooting: Force Stop Unresponsive Containers

If `docker stop` fails, you can manually stop the container using the container’s process ID:

### 1. Get the container's PID

```
docker inspect --format '{{.State.Pid}}' <container_name>
```

### 2. Kill the process

```
sudo kill -9 <PID>
```

### 3. Remove the container

```
docker rm <container_name>
```

### (Optional) Restart Docker daemon

```
sudo systemctl restart docker
```

---

## **standard Docker commands** to stop containers and clean up your environment:

---

## 🛑 Stop Docker Containers

### Stop a specific container:

```
docker stop <container_name_or_id>
```

### Stop all running containers:

```
docker stop $(docker ps -q)
```

---

## 🧼 Remove Docker Containers

### Remove a specific container:

```
docker rm <container_name_or_id>
```

### Remove all **stopped** containers:

```
docker container prune
```

### Remove all containers (running or stopped):

```
docker rm -f $(docker ps -aq)
```

---

## 🗑️ Remove Docker Images

### Remove a specific image:

```
docker rmi <image_id>
```

### Remove all unused images:

```
docker image prune
```

### Remove all images:

```
docker rmi -f $(docker images -aq)
```

---

## 🧹 Remove Docker Volumes (Optional)

### Remove unused volumes:

```
docker volume prune
```

### Remove all volumes:

```
docker volume rm $(docker volume ls -q)
```

---

## 🔄 Restart Docker (if needed)

```
sudo systemctl restart docker
```

## 🧹 Cleanup Docker Resources (Extreme Measures)

To remove all containers and images:

```
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)
docker rmi -f $(docker images -aq)
```

---

Let us know if you'd like to add **Docker Compose** for easier orchestration!
