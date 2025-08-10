---
title: "Source & Target Databases Setup"
date: 2025-08-09T00:00:00+07:00
weight: 5
chapter: false
pre: " <b> 1.5 </b> "
---

In this section, you will create **Source Database (MySQL)** and **Target Database (PostgreSQL)** in Amazon RDS.

---

### Create Source Database (MySQL)

#### Access RDS Console
- Go to **RDS Console** → Click **Create database**.


![Create Database Security Group](/images/1-env-setup/rds-cr-db.png)

#### Database Configuration
- **Database creation method:** Standard create  
- **Engine:** MySQL  
- **Version:** (latest)  
- **Template:** Free tier  

![Create Database Security Group](/images/1-env-setup/cr-db1.png)

#### Settings
- DB instance identifier: `source-mysql-db`  
- Master username: `admin`  
- Password: `MyPassword123!`  

![Create Database Security Group](/images/1-env-setup/cr-db2.png)

#### Instance Configuration
- DB instance class: `db.t3.micro` (Free tier eligible)

#### Storage
- Storage type: General Purpose SSD (gp2)  
- Allocated storage: 20 GiB  
- Disable storage autoscaling  

![Create Database Security Group](/images/1-env-setup/cr-db3.png)

#### Connectivity
- VPC: `migration-vpc`  
- DB subnet group: `migration-db-subnet-group`  
- Public access: No  
- Security group: `db-migration-sg`  

![Create Database Security Group](/images/1-env-setup/cr-db4.png)

#### Additional Configuration
- Initial database name: `sampledb`  
- Backup retention: 7 days  
- Enhanced monitoring: No

![Create Database Security Group](/images/1-env-setup/cr-db5.png)

**Click "Create database"** and wait ~10 minutes.

📝 **Notes:**
- Source DB Identifier: `source-mysql-db`
- Endpoint: *(available after creation)*
- Username: `admin`
- Password: `MyPassword123!`

---

### Create Target Database (PostgreSQL)

#### Create PostgreSQL Database
- In **RDS Console**, click **Create database**.

#### Database Configuration
- **Engine:** PostgreSQL  
- **Version:** (latest)  
- **Template:** Free tier  

#### Settings
- DB instance identifier: `target-postgres-db`  
- Master username: `postgres`  
- Password: `MyPassword123!`  

![Create Database Security Group](/images/1-env-setup/cr-db6.png)

#### Instance Configuration
- DB instance class: `db.t3.micro`

#### Storage
- Storage type: General Purpose SSD (gp2)  
- Allocated storage: 20 GiB  

#### Connectivity
- VPC: `migration-vpc`  
- DB subnet group: `migration-db-subnet-group`  
- Public access: No  
- Security group: `db-migration-sg`  

![Create Database Security Group](/images/1-env-setup/cr-db7.png)

#### Additional Configuration
- Initial database name: `targetdb`  

![Create Database Security Group](/images/1-env-setup/cr-db8.png)

**Click "Create database"** and wait ~10 minutes.

📝 **Notes:**
- Target DB Identifier: `target-postgres-db`
- Endpoint: *(available after creation)*
- Username: `postgres`
- Password: `MyPassword123!`
