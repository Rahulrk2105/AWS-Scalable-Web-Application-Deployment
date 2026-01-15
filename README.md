# AWS Scalable Web Application Deployment (Web Tier)

## 🚀 Project Overview

This repository contains a complete, step-by-step implementation of a **scalable web application architecture on Amazon Web Services (AWS)**.  
I independently designed and deployed the solution using the AWS Management Console while being mindful of the **AWS Free Tier limits**.

The architecture demonstrates:
- Secure networking (VPC, Subnets, IGW)
- Public web server hosting using EC2
- Scalability using Auto Scaling Group
- Load distribution using Classic Load Balancer
- Monitoring using Amazon CloudWatch

---

## 🏗 Architecture Diagram

![AWS Scalable Web App Architecture](./architecture/aws-scalable-web-architecture.png)

### 🧠 Architecture Explained

- **Internet** → Traffic enters from the internet
- **Internet Gateway (IGW)** → Provides inbound/outbound connectivity
- **VPC with Public Subnets** → Isolated network for resources
- **Auto Scaling Group** → Manages EC2 instance scaling
- **Classic Load Balancer** → Distributes incoming HTTP traffic
- **EC2 Web Servers** → Serve HTTP application content
- **CloudWatch** → Monitors metrics and health status

---

## ☁️ AWS Services Used

| AWS Service | Purpose |
|-------------|---------|
| VPC | Custom network with internet access |
| Subnets | Public subnets for web access |
| Internet Gateway | Enables internet connectivity |
| EC2 | Hosts Apache web server |
| Auto Scaling Group | Manages horizontal scaling |
| Classic Load Balancer | Distributes HTTP traffic to EC2 |
| CloudWatch | Monitoring and metrics |
| Security Groups | Controls inbound and outbound traffic |

---

## 🛠 Implementation Steps

### 🔹 1. Login & Region Selection
- Logged into AWS Management Console
- Selected the `ap-south-1` (Mumbai) region

---

### 🔹 2. Create VPC & Networking

- Created a **VPC** with CIDR block: `10.0.0.0/16`
- Created **two public subnets**
- Attached an **Internet Gateway**
- Configured route table with:
0.0.0.0/0 → Internet Gateway

yaml
Copy code

---

### 🔹 3. Configure Security Groups

**EC2 Security Group**
| Protocol | Port | Source |
|----------|------|--------|
| HTTP | 80 | 0.0.0.0/0 |
| SSH | 22 | My IP |

**Load Balancer Security Group**
| Protocol | Port | Source |
|----------|------|--------|
| HTTP | 80 | 0.0.0.0/0 |

---

### 🔹 4. Launch EC2 Web Server

1. Launched an Amazon Linux 2 instance (`t2.micro`)
2. Used the following **User Data** to install Apache:

```bash
#!/bin/bash
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>EC2 PUBLIC ACCESS WORKING</h1>" > /var/www/html/index.html
Verified web access via http://<Public-IP>

🔹 5. Create AMI
Created a custom Amazon Machine Image (AMI) of the configured instance

This AMI was used in the Launch Template

🔹 6. Launch Template
Created a Launch Template using the custom AMI

Included instance type, key pair, and security group

🔹 7. Auto Scaling Group
Created an Auto Scaling Group using the Launch Template

Configured:

Min capacity: 1

Desired capacity: 1

Max capacity: 2

Selected both public subnets

🔹 8. Classic Load Balancer (Temporary)
Created a Classic Load Balancer (internet-facing)

Listener: HTTP (80)

Registered Auto Scaling instances

Verified traffic distribution

⚠️ Classic Load Balancer was used due to AWS Free Tier cost constraints and deleted after verification.

🔹 9. Monitoring with CloudWatch
Monitored the following:

EC2 CPU Utilization

Load Balancer Metrics

HealthyHostCount

Auto Scaling Group Activity

Instance launch/terminate events

📊 Validation & Testing
Test	Status
Apache Running	✅
Public Web Access	✅
Auto Scaling Group Scaling	✅
Load Balancer Traffic Distribution	✅
CloudWatch Monitoring	✅

🧹 Cleanup (Important)
To avoid accumulating AWS costs, I cleaned up all resources:

Deleted Classic Load Balancer

Deleted Auto Scaling Group

Deleted Launch Template

Deregistered AMI

Terminated EC2 instance(s)

Removed VPC and subnets

🧠 Key Learnings
AWS VPC design and routing

EC2 provisioning and Apache configuration

Launch Templates and Auto Scaling Group setup

Load balancer configuration

Monitoring with Amazon CloudWatch

Cost-aware cloud design

