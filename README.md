# 🚀 Dockerized Nginx Application with Manual Load Balancer & DNS

This project demonstrates deploying an **Nginx web application using Docker** on **Ubuntu 22.04**, running the container with a **non-root user**, and manually configuring a **Load Balancer** and **DNS**.

---

## 📌 Prerequisites

- alpine 3.17 / EC2 Instance
- Docker installed
- Git installed
- Open ports: 8090, 80

---
-------------------------------
## 📂 Project Structure
---------------------------------

## 🐳 Step 1: Build Docker Image

docker build -t nginx-app:1.0 .

Step 2: Run Docker Container:

docker run -d -p 8090:8090 --name nginx-container nginx-app:1.0

Access application:

http://<server-public-ip>:8090

## ⚖️ Load Balancer Configuration (AWS ALB)

This application is exposed to the internet using an **AWS Application Load Balancer (ALB)**, which distributes traffic across multiple EC2 instances running Dockerized Nginx containers.

---

### 🧱 Architecture Flow

Client → ALB (HTTP : 80) → Target Group → EC2 (Docker Nginx : 8090)

---

### Create Target Group

1. Go to **EC2 → Target Groups → Create target group**
2. Configuration:
   - Target type: **Instance**
   - Protocol: **HTTP**
   - Port: **8090**
   - VPC: Same as EC2 instances
3. Health Check:
   - Protocol: **HTTP**
   - Path: `/`
4. Create target group and **register EC2 instances**

---

### Create Application Load Balancer

1. Go to **EC2 → Load Balancers → Create Load Balancer**
2. Select **Application Load Balancer**
3. Basic settings:
   - Name: `nginx-alb`
   - Scheme: **Internet-facing**
   - IP type: **IPv4**
4. Network:
   - Select same VPC
   - Select at least **two subnets (AZs)**
5. Security Group:
   - Allow inbound **HTTP (80)**

---

DNS Mapping with Route 53

If you own a domain:

Go to Route 53 → Hosted Zones

Create A Record

Enable Alias

Target:

Select Application Load Balancer

Save.


👤 Author

Raj Katkar
DevOps Engineer




