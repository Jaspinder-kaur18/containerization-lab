# Experiment 9: Ansible Automation with Docker

---

## Objective

To understand and implement Ansible for configuration management by automating tasks on multiple Docker-based servers using SSH authentication.

---

## Theory

### What is Ansible?

Ansible is an open-source automation tool used for:

* Configuration Management
* Application Deployment
* Orchestration

It uses an **agentless architecture**, meaning no software is required on managed nodes. Communication is done using **SSH**.

---

###  Key Features

* Agentless (uses SSH)
* Idempotent (safe repeated execution)
* YAML-based playbooks
* Push-based architecture
* Simple and human-readable

---

###  Key Components

| Component     | Description                             |
| ------------- | --------------------------------------- |
| Control Node  | Machine where Ansible is installed      |
| Managed Nodes | Target systems (Docker containers)      |
| Inventory     | List of servers (`inventory.ini`)       |
| Playbooks     | YAML files defining tasks               |
| Tasks         | Individual actions                      |
| Modules       | Built-in functions (copy, command, apt) |

---

##  Tools Used

* Ansible
* Docker
* SSH
* macOS Terminal
<img width="959" height="504" alt="image" src="https://github.com/user-attachments/assets/b7ca0f01-0145-4e60-a889-eff72bb3668b" />
<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/79c3dd4b-6dd5-44cf-972f-5887a3a17c73" />
<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/f5bbdacf-a4f3-40a7-81d2-c8270ff85d75" />
---

##  Implementation Steps

---

###  Step 1: Create Project Directory

```bash
mkdir ansible-docker-lab
cd ansible-docker-lab
```
<img width="959" height="256" alt="image" src="https://github.com/user-attachments/assets/fe23fa28-ac33-46de-bf6a-8ae03d83d061" />
---

###  Step 2: Generate SSH Keys

```bash
ssh-keygen -t rsa -b 4096
```
<img width="959" height="421" alt="image" src="https://github.com/user-attachments/assets/70614879-251a-4c51-a027-191d2efb8039" />

---

###  Step 3: Copy SSH Keys

```bash
cp ~/.ssh/id_rsa .
cp ~/.ssh/id_rsa.pub .
```
<img width="959" height="421" alt="image" src="https://github.com/user-attachments/assets/c6548f2f-1c37-4e7a-9481-e630a94e856c" />
---

###  Step 4: Create Dockerfile

```dockerfile
FROM ubuntu

RUN apt update -y
RUN apt install -y python3 python3-pip openssh-server
RUN mkdir -p /var/run/sshd

RUN mkdir -p /run/sshd && \
    echo 'root:password' | chpasswd && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config && \
    sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config

RUN mkdir -p /root/.ssh && chmod 700 /root/.ssh

COPY id_rsa /root/.ssh/id_rsa
COPY id_rsa.pub /root/.ssh/authorized_keys

RUN chmod 600 /root/.ssh/id_rsa && \
    chmod 644 /root/.ssh/authorized_keys

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
```
<img width="782" height="77" alt="image" src="https://github.com/user-attachments/assets/e692e4a1-56e7-4ef6-8a8d-fc54939329b2" />

<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/98e34699-f16f-4af2-8bcd-7f79795ec67f" />

---

###  Step 5: Build Docker Image

```bash
docker build -t ubuntu-server .
```

<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/ebcfcf4e-a237-44eb-afeb-9b69d3297a8c" />
---

###  Step 6: Run Multiple Containers

```bash
for i in {1..4}; do
  docker run -d -p 220${i}:22 --name server${i} ubuntu-server
done
```
<img width="959" height="278" alt="image" src="https://github.com/user-attachments/assets/aff0e9bb-6fdd-4a21-9f21-3814289c127f" />

<img width="958" height="502" alt="image" src="https://github.com/user-attachments/assets/ca27c63f-af0f-4988-bd4f-
---

###  Step 7: Create Inventory File (inventory.ini)

```ini
[servers]
localhost ansible_port=2201
localhost ansible_port=2202
localhost ansible_port=2203
localhost ansible_port=2204

[servers:vars]
ansible_user=root
ansible_ssh_private_key_file=~/.ssh/id_rsa
ansible_python_interpreter=/usr/bin/python3
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```
<img width="803" height="267" alt="image" src="https://github.com/user-attachments/assets/75626c47-b1dd-4556-8c24-18ef8e7cf92b" />
---

###  Step 8: Test Connectivity

```bash
ansible all -i inventory.ini -m ping
```
![images for exp 9](./images/image8.jpeg)
---

###  Step 9: Create Playbook (playbook.yml)

```yaml
---
- name: Quick Ansible Test
  hosts: all
  become: yes

  tasks:
    - name: Create test file
      copy:
        dest: /root/ansible_test.txt
        content: |
          Configured successfully using Ansible
          Host: {{ inventory_hostname }}

    - name: Get system info
      command: uname -a
      register: sysinfo

    - name: Display output
      debug:
        msg: "{{ sysinfo.stdout }}"
```
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/dd8f1762-dfe4-4005-b526-1ffbe0385e3b" />
---

###  Step 10: Run Playbook

```bash
ansible-playbook -i inventory.ini playbook.yml
```
<img width="959" height="501" alt="image" src="https://github.com/user-attachments/assets/92dc0f57-26fc-433f-8d1b-962696b25057" />

---

###  Step 11: Verify Output

```bash
ansible all -i inventory.ini -m command -a "cat /root/ansible_test.txt"
```
<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/8c170914-3f06-4bff-aa0b-14fee34c7933" />
---

###  Step 12: Cleanup

```bash
for i in {1..4}; do docker rm -f server${i}; done
```
<img width="773" height="100" alt="image" src="https://github.com/user-attachments/assets/7c331958-7106-45cb-b35f-073a226e0611" />

---

## Result

* Successfully created Docker-based servers
* Established SSH connection using key-based authentication
* Automated configuration using Ansible playbook
* Verified execution across multiple nodes

---

##  Conclusion

Ansible simplifies server management by automating repetitive tasks. It ensures consistency, reduces manual effort, and enables scalable infrastructure management using a simple YAML-based approach.

---
