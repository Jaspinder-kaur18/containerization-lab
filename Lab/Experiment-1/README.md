# Experiment 1

## Title
Compare Virtual Machine and Container

---

## Objective
To study Virtual Machines and Containers, deploy services using both approaches, analyze system resource usage, and conclude which virtualization technique is more efficient.

---

## Tools Used
- Oracle VirtualBox  
- Vagrant  
- Ubuntu (Jammy 22.04)  
- Nginx  
- Docker  
- Windows Task Manager  

---

## Theory

Virtual Machine (VM) and Container are two different approaches to virtualization.

A Virtual Machine provides hardware-level virtualization and runs a complete operating system. Each VM includes its own OS, libraries, and dependencies. Due to this, Virtual Machines consume more CPU, memory, and storage resources. VM startup time is also higher.

Containers provide operating system-level virtualization. Containers share the host operating system kernel and only package the application with its dependencies. This makes containers lightweight, faster, and more resource-efficient.

In this experiment, a Virtual Machine is created using VirtualBox and Vagrant, and Nginx is installed inside it. Later, the same service is deployed using Docker containers. Resource usage is compared to determine which approach performs better.

---

## Procedure

### Part A: Virtual Machine Setup

#### Step 1: Install VirtualBox
Oracle VirtualBox was installed to manage virtual machines.

<img width="677" height="692" alt="image1" src="https://github.com/user-attachments/assets/52949f93-6939-4754-835d-fd636359ac29" />


---

#### Step 2: Install Vagrant
Vagrant was installed to automate VM creation.

<img width="732" height="566" alt="image2" src="https://github.com/user-attachments/assets/f16372f8-df87-4325-8d2e-26ec655e61fe" />


---

#### Step 3: Verify Vagrant Installation

```bash
vagrant --version
```
<img width="1366" height="768" alt="image3" src="https://github.com/user-attachments/assets/33896eda-43f2-443e-ad52-dd7ae215cd31" />

#### Step 4: Initialize Ubuntu Virtual Machine

```bash
vagrant init ubuntu/jammy64
```
<img width="1366" height="768" alt="image4" src="https://github.com/user-attachments/assets/4aa18518-44b3-4886-a24c-6b443c1121d5" />



#### Step 5: Start the Virtual Machine
```bash
vagrant up
```
<img width="1366" height="768" alt="image5" src="https://github.com/user-attachments/assets/47e51ef0-0278-46a6-8458-9aac5ce2b6cc" />


#### Step 6: Access Ubuntu VM
```bash
vagrant ssh
```
<img width="1366" height="768" alt="image6" src="https://github.com/user-attachments/assets/facd6cd9-c6de-4e09-8fc3-7396de0a3a41" />


#### Step 7: Install Nginx inside VM
```bash
sudo apt update
sudo apt install nginx
sudo systemctl start nginx
```


#### Step 8: Verify Nginx in VM
```bash
curl localhost
```
<img width="1366" height="768" alt="image7" src="https://github.com/user-attachments/assets/b7d35605-7fa5-4224-b882-105ebccb5000" />


#### Step 9: Observe Resource Usage (VM)

CPU and memory usage were monitored using Task Manager while VM was running.
<img width="1366" height="768" alt="image8" src="https://github.com/user-attachments/assets/03c2fc87-b0ed-4f4a-8a9d-49c094cff66b" />




### Part B: Container Setup (Docker)
#### Step 10: Pull Ubuntu and Nginx Images
```bash
docker pull ubuntu
docker pull nginx
```
<img width="1366" height="768" alt="image9" src="https://github.com/user-attachments/assets/488f18f2-9a7a-4e99-ab7b-6b1012ddcbc8" />


<img width="902" height="848" alt="image10" src="https://github.com/user-attachments/assets/259d914b-4a55-4e1f-a120-c9195b029a59" />


#### Step 11: Run Nginx Container
```bash
docker run -d -p 8080:80 --name nginx-container nginx
```

#### Step 12: Verify Nginx in Container
```bash
curl localhost:8080
```


### Commands Used
```bash
vagrant init ubuntu/jammy64
vagrant up
vagrant ssh
sudo apt update
sudo apt install nginx
curl localhost
vagrant halt
vagrant destroy

docker pull ubuntu
docker pull nginx
docker run -d -p 8080:80 --name nginx-container nginx
docker images
```

### Observation

It was observed that the Virtual Machine consumed significantly higher CPU and memory resources because it runs a complete operating system. The Docker container started faster and consumed fewer resources while running the same Nginx service.
<img width="1366" height="768" alt="image11" src="https://github.com/user-attachments/assets/d1f39930-034b-4c30-bfd2-5c636acde974" />


<img width="1366" height="768" alt="image12" src="https://github.com/user-attachments/assets/f17b748a-43e5-485c-8938-c9ddfd867bb2" />

### Conclusion

Containers are more efficient than Virtual Machines. Containers are lightweight, start faster, and consume fewer system resources because they share the host operating system kernel. This experiment clearly demonstrates that container-based virtualization is better suited for modern application deployment compared to traditional Virtual Machines.

