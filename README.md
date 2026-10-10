# 🚀 AWS Blue-Green Deployment with Canary Release

![AWS](https://img.shields.io/badge/AWS-Cloud-orange)
![EC2](https://img.shields.io/badge/EC2-Compute-blue)
![ALB](https://img.shields.io/badge/Application_Load_Balancer-Traffic_Routing-success)
![Deployment](https://img.shields.io/badge/Deployment-Blue--Green-green)
![License](https://img.shields.io/badge/License-MIT-blue)

---

# 📖 Project Overview

Modern applications cannot afford downtime during deployments. Traditional deployment strategies often interrupt user access, resulting in poor user experience and potential business loss.

This project demonstrates how to perform a **Zero Downtime Blue-Green Deployment** on **Amazon Web Services (AWS)** using an **Application Load Balancer (ALB)** with **Weighted Target Groups** to implement a **Canary Release Strategy**.

Instead of replacing an existing application directly, two independent production environments are maintained:

- 🔵 Blue Environment (Current Production)
- 🟢 Green Environment (New Release)

Initially, all user traffic is routed to the Blue environment. During deployment, a small percentage of traffic is gradually shifted to the Green environment for validation. Once verified, all traffic is routed to the Green environment without any downtime.

---

# 🎯 Objectives

- Build production-like AWS infrastructure
- Deploy two isolated application environments
- Configure an Application Load Balancer
- Implement Canary Deployment (80/20 Traffic Split)
- Perform Zero Downtime Deployment
- Understand Blue-Green Deployment Strategy

---

# 🏗 Architecture

```
                    Internet
                        │
                        ▼
          +---------------------------+
          | Application Load Balancer |
          +---------------------------+
                  │             │
             80%  │             │ 20%
                  ▼             ▼

         +---------------+   +---------------+
         |   TG-Blue     |   |   TG-Green    |
         +---------------+   +---------------+
               │                    │
               ▼                    ▼
        +---------------+    +---------------+
        | Blue Server   |    | Green Server  |
        | Version 1     |    | Version 2     |
        +---------------+    +---------------+
```

After validation

```
Application Load Balancer
            │
            ▼
      100% Traffic
            │
            ▼
        TG-Green
            │
            ▼
      Green Server
```

---

# ☁ AWS Services Used

| Service | Purpose |
|----------|----------|
| Amazon EC2 | Host Blue and Green application servers |
| Amazon VPC | Isolated virtual network |
| Public Subnets | Deploy EC2 instances |
| Internet Gateway | Internet access |
| Route Tables | Network routing |
| Security Groups | Firewall configuration |
| Application Load Balancer | Traffic routing |
| Target Groups | Backend server groups |

---

# 🏛 Infrastructure

## VPC

```
Name : BG-VPC

CIDR : 10.0.0.0/16
```

---

## Public Subnets

| Subnet | CIDR | Availability Zone |
|----------|-----------|----------------|
| Public-1 | 10.0.1.0/24 | us-east-1a |
| Public-2 | 10.0.2.0/24 | us-east-1b |

---

## Internet Gateway

Attached to BG-VPC

---

## Route Table

```
Destination

0.0.0.0/0

↓

Internet Gateway
```

---

## Security Group

Allowed Inbound Rules

| Port | Protocol | Source |
|------|-----------|----------|
| 80 | HTTP | 0.0.0.0/0 |

---

# 🖥 EC2 Instances

## Blue Environment

| Property | Value |
|----------|----------|
| Name | Blue-Server |
| OS | Amazon Linux 2023 |
| Instance | t3.micro |
| Subnet | Public-1 |

Displays

```
🟦 VERSION 1 (BLUE)
```

---

## Green Environment

| Property | Value |
|----------|----------|
| Name | Green-Server |
| OS | Amazon Linux 2023 |
| Instance | t3.micro |
| Subnet | Public-2 |

Displays

```
🟩 VERSION 2 (GREEN)
```

---

# ⚙ User Data Script

Both servers automatically install Apache during launch.

Example

```bash
#!/bin/bash

yum update -y

yum install httpd -y

systemctl enable httpd

systemctl start httpd

echo "<h1>VERSION</h1>" > /var/www/html/index.html
```

---

# 🎯 Target Groups

## TG-Blue

Registered Instance

```
Blue Server
```

---

## TG-Green

Registered Instance

```
Green Server
```

---

# 🌐 Application Load Balancer

Configuration

```
Listener

HTTP : 80
```

Default Target Group

```
TG-Blue
```

---

# 🚀 Deployment Workflow

## Step 1

Create VPC

↓

Create Public Subnets

↓

Attach Internet Gateway

↓

Configure Route Table

↓

Launch EC2 Instances

↓

Install Apache

↓

Create Target Groups

↓

Create Application Load Balancer

↓

Route Traffic to Blue

---

# 🔵 Phase 1

Initial Production

```
ALB

↓

TG-Blue

↓

Blue Server
```

Traffic Distribution

```
Blue 100%

Green 0%
```

Browser Output

```
🟦 VERSION 1 (BLUE)
```

---

# 🟡 Phase 2

Canary Deployment

Traffic Weight

```
Blue : 80%

Green : 20%
```

Refreshing the browser multiple times will randomly display

```
🟦 VERSION 1

OR

🟩 VERSION 2
```

This allows testing the new version with a small percentage of users.

---

# 🟢 Phase 3

Full Cutover

Traffic Weight

```
Blue : 0%

Green : 100%
```

Now every request reaches the Green environment.

Browser

```
🟩 VERSION 2 (GREEN)
```

---

# 📊 Deployment Flow

```
Deploy New Version

↓

Launch Green Server

↓

Health Check

↓

20% Traffic

↓

Monitor Logs

↓

80% Traffic

↓

100% Traffic

↓

Deployment Complete
```

---

# ✅ Advantages of Blue-Green Deployment

- Zero Downtime
- Faster Release
- Easy Rollback
- Minimal Risk
- High Availability
- Better User Experience
- Production Validation
- Safer Deployments

# 💡 Skills Demonstrated

- Amazon EC2
- Amazon VPC
- AWS Networking
- Route Tables
- Internet Gateway
- Security Groups
- Application Load Balancer
- Target Groups
- Apache Web Server
- Linux Administration
- Blue-Green Deployment
- Canary Deployment
- Zero Downtime Deployment
- Traffic Routing
- High Availability
- Production Deployment Strategy

---

# 📚 Learning Outcomes

After completing this project, you will understand:

- Blue-Green Deployment Architecture
- Canary Release Strategy
- ALB Weighted Routing
- AWS Networking Fundamentals
- High Availability Design
- Zero Downtime Deployment Techniques
- Traffic Management using Target Groups
- Production Deployment Best Practices


## ⭐ If you found this project useful, consider giving it a Star!
