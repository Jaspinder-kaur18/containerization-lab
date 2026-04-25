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

<img width="940" height="492" alt="image" src="https://github.com/user-attachments/assets/f75adc82-c172-4177-8a17-e33f8f5221b1" />


<img width="940" height="490" alt="image" src="https://github.com/user-attachments/assets/5cbbd221-71a1-4b8c-9d01-7c9d4ea12c5c" />



Step 3: Verify Output

Open browser:
 http://localhost:8080

<img width="940" height="494" alt="image" src="https://github.com/user-attachments/assets/a0991cfe-d929-4075-a47e-086f00266667" />

Step 4: Check Image Size
docker images nginx

<img width="940" height="489" alt="image" src="https://github.com/user-attachments/assets/06326574-1e78-4061-8280-bba7d540eace" />


🔹 PART 2: UBUNTU-BASED NGINX
Step 1: Create Folder
mkdir nginx-ubuntu
cd nginx-ubuntu

<img width="940" height="490" alt="image" src="https://github.com/user-attachments/assets/50d97196-add6-405b-a18d-66ceeb9bbea6" />


Step 2: Create Dockerfile
FROM ubuntu:22.04

RUN apt-get update && \
    apt-get install -y nginx && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

<img width="940" height="489" alt="image" src="https://github.com/user-attachments/assets/520cbf23-a8c7-45a2-8b69-a39144a6c799" />


Step 3: Build Image
docker build -t nginx-ubuntu .

<img width="940" height="491" alt="image" src="https://github.com/user-attachments/assets/2326bfd7-81de-4c96-94cc-e8ee38878ebf" />


Step 4: Run Container
docker run -d --name nginx-ubuntu -p 8081:80 nginx-ubuntu

<img width="940" height="490" alt="image" src="https://github.com/user-attachments/assets/70e7bbf1-50df-42a8-9ffd-a18a08b99c46" />


Step 5: Verify Output

👉 http://localhost:8081




Step 6: Check Image Size
docker images nginx-ubuntu

<img width="940" height="488" alt="image" src="https://github.com/user-attachments/assets/479c74c1-96ea-4e83-b866-9b60025cc59d" />


🔹 PART 3: ALPINE-BASED NGINX
Step 1: Create Folder
cd ..
mkdir nginx-alpine
cd nginx-alpine

<img width="940" height="493" alt="image" src="https://github.com/user-attachments/assets/868f52a0-5a5f-4f88-8e54-11238ba21a79" />


Step 2: Create Dockerfile
FROM alpine:latest

RUN apk add --no-cache nginx

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

<img width="940" height="495" alt="image" src="https://github.com/user-attachments/assets/3b8173b3-8b5c-40ef-84aa-97f6806ae8fe" />


Step 3: Build Image
docker build -t nginx-alpine .

<img width="940" height="491" alt="image" src="https://github.com/user-attachments/assets/bb5e8402-ed2b-414d-923c-bcb7d237caaf" />


Step 4: Run Container
docker run -d --name nginx-alpine -p 8082:80 nginx-alpine

If not working (used in your case):

docker run -d --name nginx-alpine-fixed -p 8082:80 nginx:alpine

<img width="940" height="491" alt="image" src="https://github.com/user-attachments/assets/445c2c37-48c0-4b8e-9b22-85665742a745" />


Step 5: Verify Output

http://localhost:8082

<img width="940" height="488" alt="image" src="https://github.com/user-attachments/assets/47df72b7-ed53-426e-a2d9-980cb219f890" />


Step 6: Check Image Size
docker images nginx-alpine

<img width="940" height="490" alt="image" src="https://github.com/user-attachments/assets/0ed5d8f9-6b9f-401d-bff6-52584936a0c2" />


🔹 PART 4: CUSTOM HTML USING VOLUME
Step 1: Create HTML Folder
cd ..
mkdir html
cd html

<img width="940" height="491" alt="image" src="https://github.com/user-attachments/assets/e8bc0c8b-791d-44d5-a657-702a542b6bf7" />


Step 2: Create HTML File
notepad index.html
<h1>Hello from Docker NGINX </h1>
<p>This is my custom page</p>

<img width="940" height="495" alt="image" src="https://github.com/user-attachments/assets/6116d7bf-a8ac-4b09-969d-2746c70f9e29" />


Step 3: Run Container with Volume
docker run -d -p 8083:80 -v ${PWD}/html:/usr/share/nginx/html nginx

<img width="940" height="252" alt="image" src="https://github.com/user-attachments/assets/7cd23792-1fb0-4cba-8c9a-4b9eecbfbf66" />


Step 4: Verify Output

👉 http://localhost:8083

<img width="940" height="491" alt="image" src="https://github.com/user-attachments/assets/3ab120f1-6ac0-401a-84b7-01a698f6d2cc" />


🔹 IMAGE COMPARISON
Step: Compare All Images
docker images | findstr nginx

<img width="940" height="493" alt="image" src="https://github.com/user-attachments/assets/2410e10f-9d2b-4f28-93d1-5e6ea20d8410" />


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
