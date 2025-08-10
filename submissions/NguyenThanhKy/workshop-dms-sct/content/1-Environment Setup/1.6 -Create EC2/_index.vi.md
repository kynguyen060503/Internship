---
title: "Khởi chạy EC2 Bastion Host"
date: 2025-08-09T00:00:00+07:00
weight: 6
chapter: false
pre: " <b> 1.6 </b> "
---

#### Khởi chạy EC2 Instance để truy cập cơ sở dữ liệu (Bastion Host)

**Bước 1:** Truy cập EC2 Console  
1. Tìm **EC2** trong thanh tìm kiếm của AWS Console.  
2. Chọn **Launch instances**.

![Create Database Security Group](/images/1-env-setup/find-ec2.png)

**Bước 2:** Cấu hình EC2 Instance  
- **Name:** `migration-bastion`  
- **AMI:** Amazon Linux 2023  
- **Instance type:** `t2.micro`  
- **Key pair:** Tạo mới → Name: `migration-key` → **Tải về** file `.pem`.

**Bước 3:** Cấu hình mạng  
- **VPC:** `migration-vpc`  
- **Subnet:** Chọn **Public subnet** (chọn Public Subnet 1 hoặc 2).  
- **Auto-assign public IP:** **Bật (Enable)**  
- **Security group:** Tạo mới  
  - **SSH (22)**: Source = My IP  
  - **MySQL (3306)**: Source = VPC CIDR (`10.0.0.0/16`)  
  - **PostgreSQL (5432)**: Source = VPC CIDR (`10.0.0.0/16`)  

  ![Create Database Security Group](/images/1-env-setup/cr-ec2-ins.png)

  ![Create Database Security Group](/images/1-env-setup/cr-ec2-ins2.png)

**Bước 4:** Khởi chạy  
- Nhấn **Launch instance** và chờ đến khi instance ở trạng thái **running**.
