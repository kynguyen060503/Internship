---
title: "Launch EC2 Bastion Host"
date: 2025-08-09T00:00:00+07:00
weight: 6
chapter: false
pre: " <b> 1.6 </b> "
---

#### Launch EC2 Instance for Database Access (Bastion Host)

**Step 1:** Access EC2 Console  
1. Search for **EC2** in the AWS Console search bar.  
2. Click **Launch instances**.

![Create Database Security Group](/images/1-env-setup/find-ec2.png)

**Step 2:** Configure EC2 Instance  
- **Name:** `migration-bastion`  
- **AMI:** Amazon Linux 2023  
- **Instance type:** `t2.micro`  
- **Key pair:** Create a new key pair → Name: `migration-key` → **Download** the `.pem` file.

**Step 3:** Network Settings  
- **VPC:** `migration-vpc`  
- **Subnet:** Select a **Public subnet** (choose either Public Subnet 1 or 2).  
- **Auto-assign public IP:** **Enable**  
- **Security group:** Create new  
  - **SSH (22)**: Source = My IP  
  - **MySQL (3306)**: Source = VPC CIDR (`10.0.0.0/16`)  
  - **PostgreSQL (5432)**: Source = VPC CIDR (`10.0.0.0/16`)  

  ![Create Database Security Group](/images/1-env-setup/cr-ec2-ins.png)

  ![Create Database Security Group](/images/1-env-setup/cr-ec2-ins2.png)

**Step 4:** Launch  
- Click **Launch instance** and wait for the instance to become **running**.
