* Docker
* SSH
* macOS Terminal
<img width="959" height="504" alt="image" src="https://github.com/user-attachments/assets/b7ca0f01-0145-4e60-a889-eff72bb3668b" />
<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/79c3dd4b-6dd5-44cf-972f-5887a3a17c73" />
<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/f5bbdacf-a4f3-40a7-81d2-c8270ff85d75" />

---

@@ -64,15 +67,17 @@ It uses an **agentless architecture**, meaning no software is required on manage
mkdir ansible-docker-lab
cd ansible-docker-lab
```
![images for exp 9](./images/image1.jpeg)
<img width="959" height="256" alt="image" src="https://github.com/user-attachments/assets/fe23fa28-ac33-46de-bf6a-8ae03d83d061" />

---

###  Step 2: Generate SSH Keys

```bash
ssh-keygen -t rsa -b 4096
```
![images for exp 9](./images/image2.jpeg)
<img width="959" height="421" alt="image" src="https://github.com/user-attachments/assets/70614879-251a-4c51-a027-191d2efb8039" />

---

###  Step 3: Copy SSH Keys
@@ -81,6 +86,7 @@ ssh-keygen -t rsa -b 4096
cp ~/.ssh/id_rsa .
cp ~/.ssh/id_rsa.pub .
```
<img width="959" height="421" alt="image" src="https://github.com/user-attachments/assets/c6548f2f-1c37-4e7a-9481-e630a94e856c" />

---

@@ -111,6 +117,10 @@ EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
```
<img width="782" height="77" alt="image" src="https://github.com/user-attachments/assets/e692e4a1-56e7-4ef6-8a8d-fc54939329b2" />

<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/98e34699-f16f-4af2-8bcd-7f79795ec67f" />


---

@@ -119,7 +129,8 @@ CMD ["/usr/sbin/sshd", "-D"]
```bash
docker build -t ubuntu-server .
```
![images for exp 9](./images/image5.jpeg)
<img width="959" height="502" alt="image" src="https://github.com/user-attachments/assets/ebcfcf4e-a237-44eb-afeb-9b69d3297a8c" />

---

###  Step 6: Run Multiple Containers
@@ -129,7 +140,12 @@ for i in {1..4}; do
  docker run -d -p 220${i}:22 --name server${i} ubuntu-server
done
```
![images for exp 9](./images/image6.jpeg)

<img width="959" height="278" alt="image" src="https://github.com/user-attachments/assets/aff0e9bb-6fdd-4a21-9f21-3814289c127f" />

<img width="958" height="502" alt="image" src="https://github.com/user-attachments/assets/ca27c63f-af0f-4988-bd4f-


---

###  Step 7: Create Inventory File (inventory.ini)
@@ -155,7 +171,8 @@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```bash
ansible all -i inventory.ini -m ping
```
![images for exp 9](./images/image8.jpeg)
<img width="803" height="267" alt="image" src="https://github.com/user-attachments/assets/75626c47-b1dd-4556-8c24-18ef8e7cf92b" />

---

###  Step 9: Create Playbook (playbook.yml)
@@ -182,6 +199,7 @@ ansible all -i inventory.ini -m ping
      debug:
        msg: "{{ sysinfo.stdout }}"
```
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/dd8f1762-dfe4-4005-b526-1ffbe0385e3b" />

---

@@ -190,22 +208,25 @@ ansible all -i inventory.ini -m ping
```bash
ansible-playbook -i inventory.ini playbook.yml
```
![images for exp 9](./images/image10.jpeg)
<img width="959" height="501" alt="image" src="https://github.com/user-attachments/assets/92dc0f57-26fc-433f-8d1b-962696b25057" />

---

###  Step 11: Verify Output

```bash
ansible all -i inventory.ini -m command -a "cat /root/ansible_test.txt"
```
![images for exp 9](./images/image11.jpeg)
<img width="959" height="500" alt="image" src="https://github.com/user-attachments/assets/8c170914-3f06-4bff-aa0b-14fee34c7933" />

---

###  Step 12: Cleanup

```bash
for i in {1..4}; do docker rm -f server${i}; done
```
<img width="773" height="100" alt="image" src="https://github.com/user-attachments/assets/7c331958-7106-45cb-b35f-073a226e0611" />

---
