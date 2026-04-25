EXPERIMENT 3 REPORT
Deploying NGINX Using Different Base Images
🔹 AIM

To deploy NGINX using different base images (Official, Ubuntu, Alpine) and compare their image size, layers, and performance.

🔹 PREREQUISITES
Docker installed and running
Basic knowledge of Docker commands
🔹 PART 1: OFFICIAL NGINX IMAGE
Step 1: Pull NGINX Image
docker pull nginx:latest


<img width="940" height="491" alt="image" src="https://github.com/user-attachments/assets/c86ddd33-400c-43c2-8580-4b9e1319f0b3" />


Step 2: Run Container
docker run -d --name nginx-official -p 8080:80 nginx

📸 [Add Screenshot]

Step 3: Verify Output

Open browser:
👉 http://localhost:8080

📸 [Add Screenshot of NGINX Welcome Page]

Step 4: Check Image Size
docker images nginx

📸 [Add Screenshot]

🔹 PART 2: UBUNTU-BASED NGINX
Step 1: Create Folder
mkdir nginx-ubuntu
cd nginx-ubuntu

📸 [Add Screenshot]

Step 2: Create Dockerfile
FROM ubuntu:22.04

RUN apt-get update && \
    apt-get install -y nginx && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

📸 [Add Screenshot of Dockerfile]

Step 3: Build Image
docker build -t nginx-ubuntu .

📸 [Add Screenshot]

Step 4: Run Container
docker run -d --name nginx-ubuntu -p 8081:80 nginx-ubuntu

📸 [Add Screenshot]

Step 5: Verify Output

👉 http://localhost:8081

📸 [Add Screenshot]

Step 6: Check Image Size
docker images nginx-ubuntu

📸 [Add Screenshot]

🔹 PART 3: ALPINE-BASED NGINX
Step 1: Create Folder
cd ..
mkdir nginx-alpine
cd nginx-alpine

📸 [Add Screenshot]

Step 2: Create Dockerfile
FROM alpine:latest

RUN apk add --no-cache nginx

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

📸 [Add Screenshot]

Step 3: Build Image
docker build -t nginx-alpine .

📸 [Add Screenshot]

Step 4: Run Container
docker run -d --name nginx-alpine -p 8082:80 nginx-alpine

👉 If not working (used in your case):

docker run -d --name nginx-alpine-fixed -p 8082:80 nginx:alpine

📸 [Add Screenshot]

Step 5: Verify Output

👉 http://localhost:8082

📸 [Add Screenshot]

Step 6: Check Image Size
docker images nginx-alpine

📸 [Add Screenshot]

🔹 PART 4: CUSTOM HTML USING VOLUME
Step 1: Create HTML Folder
cd ..
mkdir html
cd html

📸 [Add Screenshot]

Step 2: Create HTML File
notepad index.html
<h1>Hello from Docker NGINX 🚀</h1>
<p>This is my custom page</p>

📸 [Add Screenshot]

Step 3: Run Container with Volume
docker run -d -p 8083:80 -v ${PWD}/html:/usr/share/nginx/html nginx

📸 [Add Screenshot]

Step 4: Verify Output

👉 http://localhost:8083

📸 [Add Screenshot of Custom Page]

🔹 IMAGE COMPARISON
Step: Compare All Images
docker images | findstr nginx

📸 [Add Screenshot]

Observations Table
Image Type	Size	Performance
Official	Medium	Good
Ubuntu	Large	Slow
Alpine	Very Small	Fast
🔹 CONCLUSION
Official image is best for production
Ubuntu image is useful for debugging
Alpine image is lightweight and fast
Docker images differ based on base OS and layers
🔹 RESULT

Successfully deployed NGINX using different base images and compared their size and performance.
