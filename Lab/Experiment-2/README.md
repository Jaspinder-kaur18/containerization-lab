````md

\# Experiment 2



\## Title

Docker Installation, Configuration, and Running Images



\## Name

Jaspinder Kaur



\---



\## Objective

To pull Docker images, run containers with port mapping, and manage the container lifecycle using Docker commands.



\---



\## Tools Used

\- Docker

\- Docker Desktop

\- Command Prompt / Terminal

\- Nginx Docker Image



\---



\## Theory

Docker is a containerization platform that allows applications to run in isolated environments called containers. Docker images act as templates for containers. Containers can be started, stopped, and removed easily using Docker lifecycle commands.



\---



\## Procedure



\### Step 1: Pull Docker Image

The Nginx Docker image is pulled from Docker Hub.



```bash

docker pull nginx

````



\---



\### Step 2: Run Docker Container with Port Mapping



A Docker container is started using the Nginx image with port mapping.



```bash

docker run -d -p 8080:80 nginx

```



\---



\### Step 3: Verify Running Containers



Check the list of running containers.



```bash

docker ps

```



\---



\### Step 4: Stop and Remove Docker Container



Stop and remove the running container.



```bash

docker stop <container\_id>

docker rm <container\_id>

```



\---



\### Step 5: Remove Docker Image



Remove the Docker image from the system.



```bash

docker rmi nginx

```



\---



\## Commands Used



```

docker pull nginx

docker run -d -p 8080:80 nginx

docker ps

docker stop <container\_id>

docker rm <container\_id>

docker rmi nginx

```



\---



\## Result



Docker images were successfully pulled, containers were executed with port mapping, and container lifecycle commands were performed successfully.



\---



\## Conclusion



This experiment helped in understanding Docker basics, container execution, and lifecycle management.



\---



\## Viva Questions



\*\*Q1. What is Docker?\*\*

Docker is a containerization platform used to run applications in isolated environments.



\*\*Q2. What is a Docker image?\*\*

A Docker image is a template used to create containers.



\*\*Q3. What is port mapping?\*\*

It connects container ports to host ports.



\*\*Q4. What is the command to see running containers?\*\*

docker ps



```

```



