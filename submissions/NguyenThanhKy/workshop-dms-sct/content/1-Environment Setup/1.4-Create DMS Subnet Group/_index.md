---
title: "Create DMS Subnet Group"
date: 2025-08-09T00:00:00+07:00
weight: 4
chapter: false
pre: " <b> 1.4 </b> "
---

#### Access DMS Console
1. Search for **DMS** in AWS Console.

![Create Database Security Group](/images/1-env-setup/find-dms.png)

2. Go to **Subnet groups** in the left panel.

![Create Database Security Group](/images/1-env-setup/dms-sg.png)

#### Create DMS Subnet Group
1. Click **Create subnet group**.
2. Configure:
   - **Name:** `migration-dms-subnet-group`
   - **Description:** Subnet group for DMS replication instance
   - **VPC:** `migration-vpc`
3. Add subnets: select 2 **private subnets** from `migration-vpc`.


![Create Database Security Group](/images/1-env-setup/dms-sg-cr.png)

4. Click **Create subnet group**.

#### Notes
- **DMS Subnet Group:** `migration-dms-subnet-group`
