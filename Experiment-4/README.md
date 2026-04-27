#  Experiment 4: Docker Essentials  
## Dockerfile | .dockerignore | Tagging | Publishing

---

##  Objective

To understand Docker containerization by:
- Creating a Dockerfile
- Using .dockerignore
- Building Docker images
- Running containers
- Performing multi-stage builds
- Tagging and publishing images to Docker Hub

---

#  Part 1: Create Flask Application

## Step 1: Create Project Directory

```bash
mkdir my-flask-app
cd my-flask-app
```
<img width="959" height="356" alt="image" src="https://github.com/user-attachments/assets/ecc69692-b409-4c95-8726-11c90961e085" />


---

## Step 2: Create `app.py`

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Docker!"

@app.route('/health')
def health():
    return "OK"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```
<img width="738" height="386" alt="image" src="https://github.com/user-attachments/assets/c39d696a-9eca-4668-96fb-12732ae54ec7" />

---

## Step 3: Create `requirements.txt`

```text
Flask==2.3.3
```
<img width="752" height="392" alt="image" src="https://github.com/user-attachments/assets/2317e28c-9938-4bd2-8290-731a783a7545" />

---

#  Part 2: Create Dockerfile

Create a file named `Dockerfile`:

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]
```
<img width="758" height="394" alt="image" src="https://github.com/user-attachments/assets/5b15227d-df20-48e3-9dae-ef16f66a37ba" />
<img width="959" height="505" alt="image" src="https://github.com/user-attachments/assets/61d1aebe-60e1-417f-bd2e-f58c74667c11" />


---

# Part 3: Create `.dockerignore`

```text
__pycache__/
*.pyc
*.pyo
*.pyd

.env
.venv
env/
venv/

.vscode/
.idea/

.git/
.gitignore

.DS_Store
Thumbs.db

*.log
logs/

tests/
test_*.py
```

---
<img width="959" height="501" alt="image" src="https://github.com/user-attachments/assets/5da90601-661a-4a08-ae3b-49d28626851d" />


# Part 4: Build Docker Image

```bash
docker build -t my-flask-app .
```

Check images:

```bash
docker images
```

---

<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/e5dd094b-1115-4ea3-9184-b47c4c284ac0" />
<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/3b241ddf-9b09-488c-9af8-a9bad5efc432" />



# ▶ Part 5: Run Container

```bash
docker run -d -p 5000:5000 --name flask-container my-flask-app
```
<img width="959" height="248" alt="image" src="https://github.com/user-attachments/assets/3f0df18c-20c4-43f1-8f2f-44312c326c41" />



Check running containers:

```bash
docker ps
```
<img width="959" height="191" alt="image" src="https://github.com/user-attachments/assets/06034b60-b8d6-45d9-ac81-72c898aa1da1" />


Test in browser:

```
http://localhost:5000
```
<img width="959" height="426" alt="image" src="https://github.com/user-attachments/assets/99454cc9-f757-44c8-9c48-7861077d145f" />

Test health endpoint:

```bash
curl http://localhost:5000/health
```
<img width="959" height="497" alt="image" src="https://github.com/user-attachments/assets/08a427f3-2728-4975-9356-4edce23f0725" />


---

#  Manage Container

Stop container:

```bash
docker stop flask-container
```

Start container:

```bash
docker start flask-container
```

Remove container:

```bash
docker rm -f flask-container
```
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/b7b5fcb4-aa74-4322-bbda-98b47bb5e93c" />

---

#  Part 6: Multi-Stage Build

Create `Dockerfile.multistage`:

```dockerfile
# Stage 1 - Builder
FROM python:3.9-slim AS builder

WORKDIR /app

COPY requirements.txt .

RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

RUN pip install --no-cache-dir -r requirements.txt

# Stage 2 - Final Image
FROM python:3.9-slim

WORKDIR /app

COPY --from=builder /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

COPY app.py .

RUN useradd -m -u 1000 appuser
USER appuser

EXPOSE 5000

CMD ["python", "app.py"]
```
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/a8fb5f78-4265-493d-86e7-feb925e44d23" />


Build multi-stage image:

```bash
docker build -f Dockerfile.multistage -t flask-multistage .
```

Compare sizes:

```bash
docker images
```

---
<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/073f9ad7-7f0e-4f62-9547-87ceea1d82fc" />
<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/45247e19-2547-4bea-abd9-da02023a42dc" />
<img width="959" height="437" alt="image" src="https://github.com/user-attachments/assets/9480aaee-a8ef-4fbe-adf7-dbb199d88912" />



