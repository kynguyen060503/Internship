---
title: "Create DB Subnet Group"
date: 2025-08-09T00:00:00+07:00
weight: 3
chapter: false
pre: " <b> 1.3 </b> "
---

#### Access RDS Console
1. Search for **RDS** in AWS Console.

![Find RDS](/images/1-env-setup/find-rds.png)

2. Go to **Subnet groups** in the left panel.

![Create Subnet Group](/images/1-env-setup/rds-subnet-gr.png)

#### Create DB Subnet Group
1. Click **Create DB subnet group**.
2. Configure:
   - **Name:** `migration-db-subnet-group`
   - **Description:** Subnet group for migration databases
   - **VPC:** `migration-vpc`
3. Add subnets:
   - Select 2 Availability Zones.
   - Choose 2 **private subnets** created earlier.

![Create Database Security Group](/images/1-env-setup/create-rds-sg.png)

4. Click **Create**.

#### Notes
- **DB Subnet Group:** `migration-db-subnet-group`
