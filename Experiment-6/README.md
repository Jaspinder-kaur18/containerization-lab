# Experiment 6: Comparison of Docker Run and Docker Compose

---

## Objective

To understand and compare:

- Docker Run (Imperative approach)
- Docker Compose (Declarative approach)
- Configuration mapping between Docker Run and Compose
- Single-container and multi-container deployment
- Resource limit configuration
- Using Dockerfile with Compose
- Multi-stage builds using Compose

---

# PART A – THEORY

---

## 1. Docker Run (Imperative Approach)

The `docker run` command is used to create and start containers from an image.

It requires explicit configuration using command-line flags.

### Common Flags Used

- `-p` → Port mapping  
- `-v` → Volume mounting  
- `-e` → Environment variables  
- `--network` → Network configuration  
- `--restart` → Restart policy  
- `--memory` → Memory limit  
- `--cpus` → CPU limit  
- `--name` → Container name  

### Example

```bash
docker run -d \
  --name my-nginx \
  -p 8080:80 \
  -v ./html:/usr/share/nginx/html \
  -e NGINX_HOST=localhost \
  --restart unless-stopped \
  nginx:alpine
```

This approach is imperative because the user specifies step-by-step instructions.
<img width="959" height="408" alt="image" src="https://github.com/user-attachments/assets/278b0842-5f2d-4f84-9cc5-2a401d5946e5" />

---

## 2. Docker Compose (Declarative Approach)

Docker Compose uses a YAML configuration file (`docker-compose.yml`) to define services.

Instead of multiple `docker run` commands, a single command is used:

```bash
docker compose up -d
```

This approach is declarative because the desired state of the application is defined in a configuration file.

### Equivalent Compose Configuration

```yaml
version: '3.8'

services:
  nginx:
    image: nginx:alpine
    container_name: my-nginx
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    environment:
      NGINX_HOST: localhost
    restart: unless-stopped
```
<img width="1475" height="771" alt="Screenshot 2026-04-27 151350" src="https://github.com/user-attachments/assets/86c807e2-7d05-4e9a-88ad-fdc4809f951d" />
<img width="1919" height="1008" alt="Screenshot 2026-04-27 151648" src="https://github.com/user-attachments/assets/2f14f5f9-388f-4596-9428-a8b2b8431769" />


---

## 3. Mapping: Docker Run vs Docker Compose

| Docker Run Flag | Docker Compose Equivalent |
|-----------------|---------------------------|
| `-p` | `ports:` |
| `-v` | `volumes:` |
| `-e` | `environment:` |
| `--name` | `container_name:` |
| `--network` | `networks:` |
| `--restart` | `restart:` |
| `--memory` | `deploy.resources.limits.memory` |
| `--cpus` | `deploy.resources.limits.cpus` |
| `-d` | `docker compose up -d` |

---

## 4. Advantages of Docker Compose

- Simplifies multi-container applications
- Provides reproducible configuration
- Supports version control
- Manages complete lifecycle
- Supports service scaling

Example:

```bash
docker compose up --scale web=3
```

---

# PART B – PRACTICAL TASKS

---

## Task 1: Single Container Comparison

### Using Docker Run

```bash
docker run -d \
  --name lab-nginx \
  -p 8081:80 \
  -v $(pwd)/html:/usr/share/nginx/html \
  nginx:alpine
```

Verify:

```bash
docker ps
```

Access:

http://localhost:8081

Stop and remove:

```bash
docker stop lab-nginx
docker rm lab-nginx
```
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/35c220ff-0391-4b61-9284-db4d52d22b5c" />
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/179e90fe-3341-4e4a-aeeb-a6af088e34f9" />


---

### Using Docker Compose

```yaml
version: '3.8'

services:
  nginx:
    image: nginx:alpine
    container_name: lab-nginx
    ports:
      - "8081:80"
    volumes:
      - ./html:/usr/share/nginx/html
```
<img width="959" height="505" alt="image" src="https://github.com/user-attachments/assets/b2605756-4ef6-4f48-9130-16f92a12f6b1" />
<img width="1484" height="777" alt="Screenshot 2026-04-27 151853" src="https://github.com/user-attachments/assets/6913122e-a637-4226-8d4a-e22cecd564a4" />
<img width="1919" height="1006" alt="Screenshot 2026-04-27 152213" src="https://github.com/user-attachments/assets/a309710e-b8d0-4d20-ae06-50612794e747" />


Run:

```bash
docker compose up -d
```

Verify:

```bash
docker compose ps
```

Stop:

```bash
docker compose down
```

---

## Task 2: Multi-Container Application (WordPress + MySQL)

### A. Using Docker Run

Create network:

```bash
docker network create wp-net
```
![images for exp 6](./images/image4.jpeg)
Run MySQL:

```bash
docker run -d \
  --platform linux/amd64 \
  --name mysql \
  --network wp-net \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=wordpress \
  mysql:5.7
```

Run WordPress:

```bash
docker run -d \
  --name wordpress \
  --network wp-net \
  -p 8082:80 \
  -e WORDPRESS_DB_HOST=mysql \
  -e WORDPRESS_DB_USER=root \
  -e WORDPRESS_DB_PASSWORD=secret \
  -e WORDPRESS_DB_NAME=wordpress \
  wordpress:latest
```
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/d5f8d76f-fc34-4e99-9e40-682e622b94a0" />

<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/a89182ec-8ec5-483b-aefb-a0b5eb828c11" />
<img width="959" height="114" alt="image" src="https://github.com/user-attachments/assets/eafac455-a802-436a-9739-dbe4f6fd88d3" />

Access:

http://localhost:8082

<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/93cc9b4f-1b25-490c-b581-f1ca385ece51" />


---

### B. Using Docker Compose

```yaml
version: '3.8'

services:
  mysql:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: wordpress
    volumes:
      - mysql_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    ports:
      - "8082:80"
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_PASSWORD: secret
    depends_on:
      - mysql

volumes:
  mysql_data:
```
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/6c75557c-8e56-401a-9a63-ca5089bb4d57" />
<img width="958" height="499" alt="image" src="https://github.com/user-attachments/assets/29975934-ab9b-4137-b067-5262a5bae0d6" />

<img width="950" height="383" alt="image" src="https://github.com/user-attachments/assets/d1340d3a-6216-40ea-ba5d-b11d86e9c9b9" />

Run:

```bash
docker compose up -d
```

Stop:

```bash
docker compose down -v
```
<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/75d4d2b0-b8e9-4809-b71e-69a58a10d12b" />
<img width="1919" height="1003" alt="Screenshot 2026-04-27 155713" src="https://github.com/user-attachments/assets/b907d3fe-c7c6-4aad-9d9e-cfdb4503d183" />

---

# PART C – CONVERSION & RESOURCE TASKS

---

## Task 3: Convert Docker Run to Docker Compose

Given:

```bash
docker run -d \
  --name webapp \
  -p 5000:5000 \
  -e APP_ENV=production \
  -e DEBUG=false \
  --restart unless-stopped \
  node:18-alpine
```
<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/a663832a-0b32-4949-b65e-af1fd1dc2f22" />

Compose equivalent:

```yaml
version: '3.8'

services:
  webapp:
    image: node:18-alpine
    container_name: webapp
    ports:
      - "5000:5000"
    environment:
      APP_ENV: production
      DEBUG: "false"
    restart: unless-stopped
```
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/8c6529e5-915b-4860-855a-bf6a712c85da" />

---

## Task 4: Resource Limits Conversion

Given:

```bash
docker run -d \
  --name limited-app \
  -p 9000:9000 \
  --memory="256m" \
  --cpus="0.5" \
  --restart always \
  nginx:alpine
```

Compose equivalent:

```yaml
version: '3.8'

services:
  limited-app:
    image: nginx:alpine
    container_name: limited-app
    ports:
      - "9000:9000"
    restart: always
    deploy:
      resources:
        limits:
          memory: 256m
          cpus: '0.5'
```
<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/ad47b9eb-e9b9-4b25-82ac-eab69ae74206" />
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/1781b110-c65f-48ca-aeb1-c71f2e44e698" />
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/5c67493b-c42b-4288-9748-1c33f8ad389f" />




Note:  
The `deploy` section works only in Docker Swarm mode.

---

# PART D – USING DOCKERFILE WITH COMPOSE

---

## Task 5: Replace Standard Image with Dockerfile

### app.js

```javascript
const http = require('http');

http.createServer((req, res) => {
  res.end("Docker Compose Build Lab");
}).listen(3000);
```




### Dockerfile

```dockerfile
FROM node:18-alpine

WORKDIR /app
COPY app.js .
EXPOSE 3000

CMD ["node", "app.js"]
```
<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/8b5ed07d-fbca-47c7-803a-5b58c2fab445" />


### docker-compose.yml

```yaml
version: '3.8'

services:
  nodeapp:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: custom-node-app
    ports:
      - "3000:3000"
```

Run:

```bash
docker compose up --build -d
```
![images for exp 6](./images/image17.jpeg)
---

## Difference Between image and build

| image | build |
|-------|-------|
| Uses prebuilt image | Builds locally |
| Faster startup | Customizable |
| No Dockerfile required | Dockerfile required |

---

## Task 6: Multi-Stage Dockerfile

```dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY app.js .

FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/app.js .
EXPOSE 3000
CMD ["node", "app.js"]
```

Compose configuration:

```yaml
version: '3.8'

services:
  prod-app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3001:3000"
    environment:
      NODE_ENV: production
```
<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/7d9eb467-e26d-48f4-abbb-5acffb8eb3ad" />
<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/2248b199-94af-4d1d-adc1-c79540905c06" />
<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/1b259f36-0a94-4326-bf92-29565a2c14fb" />
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/5ce287f2-7266-4f13-b7b7-efba419bb451" />
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/3334dfeb-ee22-4fb5-a721-fb46b4da6bb0" />

# Conclusion

Docker Run follows an imperative approach where configuration is provided manually through flags.

Docker Compose follows a declarative approach where the entire application configuration is defined in a YAML file.

Docker Compose is preferred for multi-container and scalable applications, while Docker Run is suitable for simple and quick container execution.
