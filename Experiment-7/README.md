# CI/CD Pipeline using Jenkins, GitHub & Docker Hub
---

#  Aim

To design and implement a complete CI/CD pipeline using Jenkins by integrating GitHub and Docker Hub.

---

#  Objectives

* Understand CI/CD workflow
* Automate build and deployment
* Integrate GitHub, Jenkins, and Docker Hub
* Use Webhooks for automation

---

#  Tools Used

* Jenkins
* GitHub
* Docker
* Docker Hub
* Flask

---

#  Project Structure

```
my-app/
├── app.py
├── requirements.txt
├── Dockerfile
├── Jenkinsfile
```

---

#  Step-by-Step Implementation

---

##  Step 1: Create Project in VS Code

* Created folder `my-app`
* Added files:

  * `app.py`
  * `requirements.txt`
  * `Dockerfile`
  * `Jenkinsfile`

<img width="745" height="391" alt="image" src="https://github.com/user-attachments/assets/ed3234a0-5834-4de1-b93b-2de055c6f805" />
<img width="760" height="395" alt="image" src="https://github.com/user-attachments/assets/bd39d0a6-61ab-416a-a120-2a74ef9a263b" />
<img width="934" height="503" alt="image" src="https://github.com/user-attachments/assets/12ae96c3-59b8-4a82-8486-f66d3b84f60e" />




---

##  Step 2: Write Application Code

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "CI/CD is working 🚀 - Jaspinder FINAL"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=80)
```

<img width="758" height="395" alt="image" src="https://github.com/user-attachments/assets/f9115951-9fe7-450e-944c-bd7e8f2cdd5d" />

---

##  Step 3: Create GitHub Repository

* Created repo: `my-app`
* Pushed code using Git commands

```bash
git init
git add .
git commit -m "initial commit"
git push
```

<img width="821" height="241" alt="image" src="https://github.com/user-attachments/assets/a86c0d47-03a5-44a2-91bf-359ec70798f9" />


---

## Step 4: Setup Jenkins using Docker

* Created `docker-compose.yml`
* Started Jenkins container

```bash
docker-compose up -d
```

* Accessed Jenkins at:
  `http://localhost:8080`

<img width="749" height="395" alt="image" src="https://github.com/user-attachments/assets/6b979ebb-1ec1-41d7-9a93-d2c88ef6a6c3" />
<img width="959" height="167" alt="image" src="https://github.com/user-attachments/assets/043bc16e-9e79-488d-a498-a39d1aa7c94a" />
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/ec779ffb-5011-4ac1-bfea-cc6e986d1198" />





---

## Step 5: Configure Jenkins

### ➤ Add Credentials

* Added Docker Hub token
* ID: `dockerhub-token`



### ➤ Create Pipeline Job

* Selected: **Pipeline script from SCM**
* Repo URL: `https://github.com/jaspinder-kaur18/my-app.git`
* Script Path: `Jenkinsfile`
<img width="959" height="506" alt="image" src="https://github.com/user-attachments/assets/b954f275-7e0d-4361-8665-de9288f4af7e" />
---

##  Step 6: Setup Webhook

* Added webhook in GitHub
* Payload URL:

```
https://your-ngrok-url/github-webhook/
```
<img width="959" height="505" alt="image" src="https://github.com/user-attachments/assets/a97f0aa6-e29b-41ae-ad4c-6a6bd5bde2b6" />
* Enabled push events

---

##  Step 7: Execute Pipeline

* Pushed code to GitHub
* Jenkins triggered automatically

### Stages executed:

* Clone Source
* Build Docker Image
* Login to Docker Hub
* Push Image
<img width="959" height="505" alt="image" src="https://github.com/user-attachments/assets/7946b9ec-9c44-46e4-a957-5b6250eb96ba" />

---

## Step 8: Verify on Docker Hub


* Tag: `latest`

<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/eba46ada-a00a-4f93-bb4d-3743a4866f23" />
---

## Step 9: Run Docker Container

```bash
docker run -d -p 8081:80 jaspinder-kaur18/myapp
```

Open:

```
http://localhost:8081
```
<img width="1440" height="1484" alt="image16(1)" src="https://github.com/user-attachments/assets/162f7a2c-0e75-4dbe-ae1f-4dc66967d19b" />

---

#  Workflow Diagram

```
Developer → GitHub → Webhook → Jenkins → Docker → Docker Hub
```

---

#  Observations

* Automation reduces manual work
* Jenkins simplifies pipeline creation
* Docker ensures consistency
* Webhook enables real-time triggering

---

# Result

Successfully implemented a CI/CD pipeline where:

* Code push triggers Jenkins automatically
* Docker image is built and pushed
* Full automation achieved

---


# Conclusion

This experiment demonstrates how modern DevOps tools can automate software development and deployment efficiently.

---
