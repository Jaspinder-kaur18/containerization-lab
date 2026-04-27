# Experiment 2

## Title
Docker Installation, Configuration, and Running Images

---

## Objective
To pull Docker images, run containers with port mapping, and manage the container lifecycle using Docker commands.

---

## Tools Used
- Docker
- Docker Desktop
- macOS Terminal
- Nginx Docker Image

---

## Theory
Docker is a containerization platform that allows applications to run in isolated environments called containers. Docker images act as templates for containers. Containers can be started, stopped, and removed easily using Docker lifecycle commands.

---

## Procedure

### Step 1: Pull Docker Image
The Nginx Docker image was pulled from Docker Hub.

---
<img width="1920" height="1080" alt="Screenshot (492)" src="https://github.com/user-attachments/assets/779ca4d9-f31a-4165-971c-75a22c833a17" />


### Step 2: Run Docker Container with Port Mapping
A Docker container was started using the Nginx image with port mapping.

---
<img width="1920" height="1080" alt="Screenshot (493)" src="https://github.com/user-attachments/assets/f470d77a-12b6-4d1b-af87-5c6cf7ae8ef7" />


### Step 3: Verify Running Containers
The list of running containers was checked.

---
<img width="1920" height="1080" alt="Screenshot (494)" src="https://github.com/user-attachments/assets/6b7e4a98-4f3a-4b47-8d73-5c1cb98ca5bd" />

<img width="1920" height="1080" alt="Screenshot (495)" src="https://github.com/user-attachments/assets/90485786-5abb-4a36-9875-6a1cfd93201e" />

### Step 4: Stop and Remove Docker Container
The running container was stopped and removed.

---
<img width="1920" height="1080" alt="Screenshot (501)" src="https://github.com/user-attachments/assets/32a9579c-2ef0-4a93-af56-5128dfb725e0" />


### Step 5: Remove Docker Image
The Docker image was removed from the system.

---
<img width="1920" height="1080" alt="Screenshot (513)" src="https://github.com/user-attachments/assets/37fc0af9-e0ec-451b-b4e9-b25dde66fc45" />

## Commands Used

```bash
docker pull nginx
docker run -d -p 8080:80 nginx
docker ps
docker stop <container_id>
docker rm <container_id>
docker rmi nginx
```

## Result

Docker images were successfully pulled, containers were executed with port mapping, and container lifecycle commands were performed successfully.
<img width="1920" height="1080" alt="Screenshot (517)" src="https://github.com/user-attachments/assets/ba6f1f0a-fdb3-44d4-abb9-c331e0c02768" />
