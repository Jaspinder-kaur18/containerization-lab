````md

\# Experiment 3



\## Title

Deploying Web Applications with Docker



\## Name

Jaspinder Kaur



\---



\## Objective

This experiment demonstrates how to deploy a simple web application using Docker.  

A Flask-based web app is containerized using a Dockerfile, built into an image, and deployed inside a running container.



\---



\## Technologies Used

\- Python (Flask)

\- Docker

\- Command Prompt / Terminal



\---



\## Theory

Docker allows applications to be packaged along with their dependencies into containers.  

Using Docker, web applications can be deployed consistently across different environments without dependency issues.



\---



\## Procedure



\### Step 1: Create Project Folder

```bash

mkdir Experiment-3-WebApp

cd Experiment-3-WebApp

````



\---



\### Step 2: Create Flask App (app.py)



```python id="5n1xwz"

from flask import Flask



app = Flask(\_\_name\_\_)



@app.route('/')

def home():

&#x20;   return """

&#x20;   <h1>Experiment 3: Deploying Web Applications with Docker</h1>

&#x20;   <h2>Jaspinder Kaur</h2>

&#x20;   """



if \_\_name\_\_ == "\_\_main\_\_":

&#x20;   app.run(host="0.0.0.0", port=5000)

```



!\[Flask Code](images/image1.png)



\---



\### Step 3: Create Requirements File



Create `requirements.txt`:



```text id="b2kq6p"

flask

```



!\[Requirements File](images/image2.png)



\---



\### Step 4: Write Dockerfile



```Dockerfile id="3n4z8k"

FROM python:3.9-slim



WORKDIR /app



COPY . /app



RUN pip install -r requirements.txt



CMD \["python", "app.py"]

```



!\[Dockerfile](images/image3.png)



\---



\### Step 5: Build Docker Image



```bash id="xv3l9p"

docker build -t experiment3-webapp .

```



!\[Build Image](images/image4.png)



\---



\### Step 6: Run Docker Container



```bash id="q2p9dl"

docker run -d -p 8080:5000 experiment3-webapp

```



!\[Run Container](images/image5.png)



\---



\### Step 7: Verify Deployment



Open in browser:



```text id="c6fzqk"

http://localhost:8080

```



!\[Output](images/image6.png)



\---



\### Step 8: Check Logs



```bash id="yq8m2h"

docker logs <container\_id>

```



\---



\### Step 9: Stop Container



```bash id="f9u2rs"

docker stop <container\_id>

```



\---



\## Commands Used



```bash id="2v7c0j"

docker build -t experiment3-webapp .

docker run -d -p 8080:5000 experiment3-webapp

docker logs <container\_id>

docker stop <container\_id>

```



\---



\## Result



The Flask web application was successfully containerized and deployed using Docker.

It ran correctly inside a Docker container and was accessible through a web browser.



\---



\## Conclusion



Containerizing web applications ensures portability, consistency, and ease of deployment across environments.

Docker simplifies packaging and deployment without dependency issues.



\---



\## Viva Questions



\*\*Q1. What is Flask?\*\*

Flask is a lightweight Python web framework.



\*\*Q2. Why use Docker for web apps?\*\*

To ensure consistent deployment across environments.



\*\*Q3. What does Dockerfile do?\*\*

It defines steps to build a Docker image.



\*\*Q4. What is port mapping?\*\*

It connects container ports to host ports.



```

```



