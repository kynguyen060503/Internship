---
title: "Create Security Groups"
date: 2025-08-09T00:00:00+07:00
weight: 2
chapter: false
pre: " <b> 1.2 </b> "
---

#### Database Security Group
1. In **VPC Console**, go to **Security Groups** → **Create security group**.

![Create SC](/images/1-env-setup/create-sc-gr.png)

2. Configure:
   - **Name:** `db-migration-sg`
   - **Description:** Security group for database migration
   - **VPC:** `migration-vpc`
3. Add inbound rules:
   - **MySQL/Aurora (3306)** → Source: `10.0.0.0/16`
   - **PostgreSQL (5432)** → Source: `10.0.0.0/16`

![Create Database Security Group](/images/1-env-setup/create-db-sg.png)

4. Click **Create security group**.

#### DMS Security Group
1. Click **Create security group** again.
2. Configure:
   - **Name:** `dms-replication-sg`
   - **Description:** Security group for DMS replication instance
   - **VPC:** `migration-vpc`
3. Inbound rules: leave empty (DMS only needs outbound).
4. Outbound rules: keep default (**All traffic**).

![Create DMS Security Group](/images/1-env-setup/create-dms-sg.png)

5. Click **Create security group**.

#### Notes
- **Database SG ID:** `sg-xxxxxxxxx`
- **DMS SG ID:** `sg-xxxxxxxxx`
