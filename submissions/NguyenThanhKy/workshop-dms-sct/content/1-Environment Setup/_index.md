---
title: "Environment Setup"
date: 2025-08-09T00:00:00+07:00
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

**Content:**
- [Create VPC and Networking](#11-create-vpc-and-networking)
- [Create Security Groups](#12-create-security-group)
- [Create RDS Subnet Group](#13-create-rds-subnet-group)
- [Create DMS Subnet Group](#14-create-dms-subnet-group)
- [Source & Target Database Group](#15-source--target-databases-setup)
- [Create EC2](#16-launch-ec2-bastion-host)
- [Connect EC2 and launch data](#17-connect-and-setup-sample-data)
- [DMS Setup](#18-create-dms-replication-instance-and-endpoints)

---

### 1.1 Create VPC and Networking

#### Access VPC Console
1. Sign in to **AWS Console**.

![VPC Console](/images/1-env-setup/aws-console.png)

2. Search for **VPC** in the search bar.

![Find VPC](/images/1-env-setup/find-vpc.png)

3. Click **VPC** service.

#### Create a New VPC
1. Click **Create VPC** → select **VPC and more**.

![Create VPC](/images/1-env-setup/create-vpc.png)

2. Configure:
   - **Name tag:** `migration-vpc`
   - **IPv4 CIDR:** `10.0.0.0/16`
   - **Number of AZs:** `2`
   - **Public subnets:** `2`
   - **Private subnets:** `2`
   - **NAT gateways:** `In 1 AZ`
   - **VPC endpoints:** `None`
   
![Create VPC1](/images/1-env-setup/create-vpc1.png)

![Create VPC2](/images/1-env-setup/create-vpc2.png)


3. Click **Create VPC** and wait ~5 minutes.

![Create VPC3](/images/1-env-setup/create-vpc3.png)

#### Notes
- **VPC ID:** `vpc-xxxxxxxxx`
- **Public Subnet 1:** `subnet-xxxxxxxxx`
- **Public Subnet 2:** `subnet-xxxxxxxxx`
- **Private Subnet 1:** `subnet-xxxxxxxxx`
- **Private Subnet 2:** `subnet-xxxxxxxxx`

---

### 1.2 Create Security Group

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

---
### 1.3 Create RDS Subnet Group

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
---
### 1.4 Create DMS Subnet Group

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

---

### 1.5 Source & Target Databases Setup

#### Access RDS Console
- Go to **RDS Console** → Click **Create database**.


![Create Database Security Group](/images/1-env-setup/rds-cr-db.png)

##### Database Configuration
- **Database creation method:** Standard create  
- **Engine:** MySQL  
- **Version:** (latest)  
- **Template:** Free tier  

![Create Database Security Group](/images/1-env-setup/cr-db1.png)

##### Settings
- DB instance identifier: `source-mysql-db`  
- Master username: `admin`  
- Password: `MyPassword123!`  

![Create Database Security Group](/images/1-env-setup/cr-db2.png)

##### Instance Configuration
- DB instance class: `db.t3.micro` (Free tier eligible)

##### Storage
- Storage type: General Purpose SSD (gp2)  
- Allocated storage: 20 GiB  
- Disable storage autoscaling  

![Create Database Security Group](/images/1-env-setup/cr-db3.png)

##### Connectivity
- VPC: `migration-vpc`  
- DB subnet group: `migration-db-subnet-group`  
- Public access: No  
- Security group: `db-migration-sg`  

![Create Database Security Group](/images/1-env-setup/cr-db4.png)

##### Additional Configuration
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

#### Create Target Database (PostgreSQL)

##### Create PostgreSQL Database
- In **RDS Console**, click **Create database**.

##### Database Configuration
- **Engine:** PostgreSQL  
- **Version:** (latest)  
- **Template:** Free tier  

##### Settings
- DB instance identifier: `target-postgres-db`  
- Master username: `postgres`  
- Password: `MyPassword123!`  

![Create Database Security Group](/images/1-env-setup/cr-db6.png)

##### Instance Configuration
- DB instance class: `db.t3.micro`

##### Storage
- Storage type: General Purpose SSD (gp2)  
- Allocated storage: 20 GiB  

##### Connectivity
- VPC: `migration-vpc`  
- DB subnet group: `migration-db-subnet-group`  
- Public access: No  
- Security group: `db-migration-sg`  

![Create Database Security Group](/images/1-env-setup/cr-db7.png)

##### Additional Configuration
- Initial database name: `targetdb`  

![Create Database Security Group](/images/1-env-setup/cr-db8.png)

**Click "Create database"** and wait ~10 minutes.

📝 **Notes:**
- Target DB Identifier: `target-postgres-db`
- Endpoint: *(available after creation)*
- Username: `postgres`
- Password: `MyPassword123!`

---
### 1.6 Launch EC2 Bastion Host

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
---
### 1.7 Connect and Setup Sample Data
#### Connect to EC2 and Setup Sample Data 

**Step 1:** Connect to EC2 Instance  
1. Wait for the EC2 instance to be in **Running** state.  
2. Copy **Public IPV4** address 
3. Open your **cmd** or **Powershell**
4. Move to folder have file migration-key.pem
```bash
cd C:\path\to\your\key\
```
5. Run this command 
```bash
ssh -i migration-key.pem ec2-user@[Public IPV4]
```
**Step 2:** Install MySQL Client  
```bash
sudo yum update -y
sudo dnf install mariadb105
```
**Step3:** Create Sample Data in MySQL
Connect to the MySQL database:

```bash
mysql -h [SOURCE-MYSQL-ENDPOINT] -u admin -p
# Enter password: MyPassword123! 
```
![Create Database Security Group](/images/1-env-setup/connect-database.png)

**Step4** Inside MySQL, create the sample tables and insert sample data:

```sql
USE sampledb;

CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    hire_date DATE
);

INSERT INTO employees VALUES
(1, 'John', 'Doe', 'john.doe@company.com', 'IT', 75000.00, '2022-01-15'),
(2, 'Jane', 'Smith', 'jane.smith@company.com', 'HR', 65000.00, '2022-02-20'),
(3, 'Mike', 'Johnson', 'mike.johnson@company.com', 'Finance', 80000.00, '2022-03-10'),
(4, 'Sarah', 'Wilson', 'sarah.wilson@company.com', 'IT', 78000.00, '2022-04-05'),
(5, 'David', 'Brown', 'david.brown@company.com', 'Marketing', 70000.00, '2022-05-12');

CREATE TABLE departments (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    manager_id INT,
    budget DECIMAL(12,2)
);

INSERT INTO departments VALUES
(1, 'IT', 1, 500000.00),
(2, 'HR', 2, 300000.00),
(3, 'Finance', 3, 400000.00),
(4, 'Marketing', 5, 350000.00);

-- Verify data
SELECT COUNT(*) FROM employees;
SELECT COUNT(*) FROM departments;
```

**Step5:** Finally, exit MySQL:

```sql
EXIT;
```

![Create Database Security Group](/images/1-env-setup/complete-db.png)

---

### 1.8 Create DMS Replication Instance and Endpoints

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


