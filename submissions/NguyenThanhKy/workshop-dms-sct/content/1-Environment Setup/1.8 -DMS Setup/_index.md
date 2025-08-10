---
title: "Create DMS Replication Instance and Endpoints"
date: 2025-08-09T00:00:00+07:00
weight: 8
chapter: false
pre: " <b> 1.8 </b> "
---

## Create DMS Replication Instance and Endpoints

### Create DMS Replication Instance

#### Access DMS Console
- Search for **"DMS"** in the AWS Management Console search bar.
- Click **Provisioned instances**.

#### Create Replication Instance
- Click **Create replication instance**.
![Create Database Security Group](/images/1-env-setup/dms-rev-ins.png)
- Configure:
  - **Name:** `migration-replication-instance`
  - **Description:** `Replication instance for database migration`
  - **Instance class:** `dms.t3.micro`
  - **Engine version:** `(latest)`
  - **Multi-AZ:** `No` (for cost saving)
![Create Database Security Group](/images/1-env-setup/cr-dms-rev-ins.png)
#### Connectivity and Security
- **VPC:** `migration-vpc`
- **Replication subnet group:** `migration-dms-subnet-group`
- **Publicly accessible:** `No`
- **VPC security group:** `dms-replication-sg`
![Create Database Security Group](/images/1-env-setup/connect-and-security.png)
Click **Create replication instance** and wait about **10 minutes** until the status is **Available**.

> 📝 **Note:**  
> Replication Instance: `migration-replication-instance`

---

### Create Source Endpoint

#### Access Endpoints
- In the DMS Console, click **Endpoints**.
- Click **Create endpoint**.
![Create Database Security Group](/images/1-env-setup/cre-endp.png)
#### Endpoint Configuration
- **Endpoint type:** `Source endpoint`
- **Endpoint identifier:** `source-mysql-endpoint`
- **Source engine:** `MySQL`
![Create Database Security Group](/images/1-env-setup/src-endp.png)
#### Access to Source Database
- **Server name:** `[SOURCE-MYSQL-ENDPOINT]`(copy from RDS -> Database -> source-mysql-db -> Connectivity & Security)
![Create Database Security Group](/images/1-env-setup/rds-endpoint.png)
- **Port:** `3306`
- **Username:** `admin`
- **Password:** `MyPassword123!`
![Create Database Security Group](/images/1-env-setup/src-endp2.png)
#### Test Endpoint Connection
- Click **Run test**.
- **VPC:** `migration-vpc`
- **Replication instance:** `migration-replication-instance`
- Click **Run test** again and wait until status is **successful**.

![Create Database Security Group](/images/1-env-setup/test-endp.png)

#### Advanced Settings
- **Extra connection attributes:** *(leave blank)*

Click **Create endpoint**.

---

### Create Target Endpoint

#### Create Endpoint
- Click **Create endpoint**.

#### Endpoint Configuration
- **Endpoint type:** `Target endpoint`
- **Endpoint identifier:** `target-postgres-endpoint`
- **Target engine:** `PostgreSQL`
![Create Database Security Group](/images/1-env-setup/targer-endp.png)
#### Access to Target Database
- **Server name:** `[TARGET-POSTGRES-ENDPOINT]`
- **Port:** `5432`
- **Username:** `postgres`
- **Password:** `MyPassword123!`
- **Database name:** `targetdb`
![Create Database Security Group](/images/1-env-setup/target-endp.png)
#### Test Endpoint Connection
- Click **Run test**.
- **Replication instance:** `migration-replication-instance`
- Click **Run test** again and wait until status is **successful**.
- Click **Create endpoint**.

> 📝 **Note:**  
> If tesing endpoint failed, go to **db-migration-sg** and add inbound rule: **PostgreSQL (5432)** → Source: (id of **dms-replication-sg**)

