---
title : "Chuẩn bị môi trường"
date: 2025-08-09T00:00:00+07:00
weight : 1
chapter : false
pre : " <b> 1. </b> "
---

**Content:**
- [Tạo VPC và cấu hình mạng](#11-tạo-vpc-và-cấu-hình-mạng)
- [Tạo Security Group](#12-tạo-security-group)
- [Tạo RDS Subnet Group](#13-tạo-db-subnet-group)
- [Tạo DMS Subnet Group](#14-tạo-dms-subnet-group)
- [Tạo Database nguồn và đích](#15-cài-đặt-cơ-sở-dữ-liệu-nguồn--đích)
- [Tạo EC2](#16-tạo-ec2))
- [Kết nối EC2 và tạo dữ liêu mẫu](#17-kết-nối-ec2-và-tạo-dữ-liệu-mẫu)
- [Cấu hình DMS](#18-cấu-hình-dms)

---
### 1.1 Tạo VPC và cấu hình mạng

#### Truy cập VPC Console
1. Đăng nhập vào **AWS Management Console**.

![VPC Console](/images/1-env-setup/aws-console.png)

2. Tìm kiếm **VPC** trong thanh tìm kiếm.

![Find VPC](/images/1-env-setup/find-vpc.png)

3. Nhấn chọn dịch vụ **VPC**.

---

#### Tạo VPC mới
1. Chọn **Create VPC** → chọn **VPC and more**.

![Create VPC](/images/1-env-setup/create-vpc.png)

2. Cấu hình:
   - **Name tag:** `migration-vpc`
   - **IPv4 CIDR:** `10.0.0.0/16`
   - **Number of AZs:** `2`
   - **Public subnets:** `2`
   - **Private subnets:** `2`
   - **NAT gateways:** `In 1 AZ`
   - **VPC endpoints:** `None`
   
![Create VPC1](/images/1-env-setup/create-vpc1.png)

![Create VPC2](/images/1-env-setup/create-vpc2.png)

3. Nhấn **Create VPC** và chờ khoảng 5 phút để AWS khởi tạo.

![Create VPC3](/images/1-env-setup/create-vpc3.png)

---

#### Ghi chú quan trọng
Sau khi tạo xong, lưu lại các thông tin sau để sử dụng trong các bước tiếp theo:

- **VPC ID:** `vpc-xxxxxxxxx` *(ID VPC vừa tạo)*
- **Public Subnet 1 ID:** `subnet-xxxxxxxxx`
- **Public Subnet 2 ID:** `subnet-xxxxxxxxx`
- **Private Subnet 1 ID:** `subnet-xxxxxxxxx`
- **Private Subnet 2 ID:** `subnet-xxxxxxxxx`

> 💡 **Tip:** Bạn có thể tìm các ID này trong trang **Subnets** và **VPCs** của AWS Console.

---
### 1.2 Tạo Security Group
#### Security Group cho Database
1. Trong **VPC Console**, vào **Security Groups** → **Create security group**.

![Create SC](/images/1-env-setup/create-sc-gr.png)

2. Cấu hình:
   - **Name:** `db-migration-sg`
   - **Description:** Security group cho quá trình di chuyển cơ sở dữ liệu
   - **VPC:** `migration-vpc`
3. Thêm **Inbound rules**:
   - **MySQL/Aurora (3306)** → Source: `10.0.0.0/16`
   - **PostgreSQL (5432)** → Source: `10.0.0.0/16`

![Create Database Security Group](/images/1-env-setup/create-db-sg.png)

4. Nhấn **Create security group**.

---

#### Security Group cho DMS
1. Nhấn **Create security group** một lần nữa.
2. Cấu hình:
   - **Name:** `dms-replication-sg`
   - **Description:** Security group cho DMS replication instance
   - **VPC:** `migration-vpc`
3. **Inbound rules:** để trống (DMS chỉ cần outbound).
4. **Outbound rules:** giữ mặc định (**All traffic**).

![Create DMS Security Group](/images/1-env-setup/create-dms-sg.png)

5. Nhấn **Create security group**.

---

#### Ghi chú quan trọng
Sau khi tạo xong, lưu lại các thông tin sau để sử dụng cho các bước tiếp theo:

- **Database SG ID:** `sg-xxxxxxxxx` *(ID của db-migration-sg)*
- **DMS SG ID:** `sg-xxxxxxxxx` *(ID của dms-replication-sg)*

> 💡 **Mẹo:** Bạn có thể tìm ID của Security Group trong cột **Group ID** ở trang **Security Groups** trong AWS Console.
---
### 1.3 Tạo DB Subnet Group

#### Truy cập RDS Console
1. Trong AWS Console, tìm **RDS**.

![Find RDS](/images/1-env-setup/find-rds.png)

2. Ở menu bên trái, chọn **Subnet groups**.

![Create Subnet Group](/images/1-env-setup/rds-subnet-gr.png)

---

#### Tạo DB Subnet Group
1. Nhấn **Create DB subnet group**.
2. Cấu hình:
   - **Name:** `migration-db-subnet-group`
   - **Description:** Subnet group cho cơ sở dữ liệu dùng trong quá trình migration
   - **VPC:** `migration-vpc`
3. Thêm subnet:
   - Chọn **2 Availability Zones**.
   - Chọn **2 private subnets** đã tạo ở bước trước.

![Create Database Security Group](/images/1-env-setup/create-rds-sg.png)

4. Nhấn **Create** để hoàn tất.

---

#### Ghi chú quan trọng
- **Tên DB Subnet Group:** `migration-db-subnet-group` *(sẽ dùng khi tạo RDS instance)*
- Đảm bảo subnets được chọn là **Private Subnets** để cơ sở dữ liệu không bị truy cập trực tiếp từ internet.

---
### 1.4 Tạo DMS Subnet Group

---
title: "Tạo DMS Subnet Group"
date: 2025-08-09T00:00:00+07:00
weight: 4
chapter: false
pre: " <b> 1.4 </b> "
---

#### Truy cập DMS Console
1. Trong AWS Console, tìm **DMS**.

![Create Database Security Group](/images/1-env-setup/find-dms.png)

2. Ở menu bên trái, chọn **Subnet groups**.

![Create Database Security Group](/images/1-env-setup/dms-sg.png)

---

#### Tạo DMS Subnet Group
1. Nhấn **Create subnet group**.
2. Cấu hình:
   - **Name:** `migration-dms-subnet-group`
   - **Description:** Subnet group cho DMS replication instance
   - **VPC:** `migration-vpc`
3. Thêm subnet:
   - Chọn **2 private subnets** thuộc `migration-vpc`.

![Create Database Security Group](/images/1-env-setup/dms-sg-cr.png)

4. Nhấn **Create subnet group**.

---

#### Ghi chú quan trọng
- **Tên DMS Subnet Group:** `migration-dms-subnet-group` *(sẽ dùng khi tạo DMS replication instance)*
- Đảm bảo đã chọn **Private Subnets** để DMS hoạt động trong mạng nội bộ và tăng tính bảo mật.
---
### 1.5 Cài đặt Cơ sở dữ liệu Nguồn & Đích
### Tạo Cơ sở dữ liệu Nguồn (MySQL)

#### Truy cập RDS Console
- Mở **RDS Console** → Chọn **Create database**.

![Create Database Security Group](/images/1-env-setup/rds-cr-db.png)

#### Cấu hình cơ sở dữ liệu
- **Database creation method:** Standard create  
- **Engine:** MySQL  
- **Version:** (phiên bản mới nhất)  
- **Template:** Free tier  

![Create Database Security Group](/images/1-env-setup/cr-db1.png)

#### Thiết lập
- DB instance identifier: `source-mysql-db`  
- Master username: `admin`  
- Password: `MyPassword123!`  

![Create Database Security Group](/images/1-env-setup/cr-db2.png)

#### Cấu hình Instance
- DB instance class: `db.t3.micro` (Free tier eligible)

#### Lưu trữ
- Storage type: General Purpose SSD (gp2)  
- Allocated storage: 20 GiB  
- Tắt tính năng storage autoscaling  

![Create Database Security Group](/images/1-env-setup/cr-db3.png)

#### Kết nối mạng
- VPC: `migration-vpc`  
- DB subnet group: `migration-db-subnet-group`  
- Public access: No  
- Security group: `db-migration-sg`  

![Create Database Security Group](/images/1-env-setup/cr-db4.png)

#### Cấu hình bổ sung
- Initial database name: `sampledb`  
- Backup retention: 7 ngày  
- Enhanced monitoring: No

![Create Database Security Group](/images/1-env-setup/cr-db5.png)

**Nhấn "Create database"** và chờ khoảng 10 phút.

📝 **Ghi chú:**
- Source DB Identifier: `source-mysql-db`
- Endpoint: *(có sau khi tạo xong)*
- Username: `admin`
- Password: `MyPassword123!`

---

### Tạo Cơ sở dữ liệu Đích (PostgreSQL)

#### Tạo cơ sở dữ liệu PostgreSQL
- Trong **RDS Console**, chọn **Create database**.

#### Cấu hình cơ sở dữ liệu
- **Engine:** PostgreSQL  
- **Version:** (phiên bản mới nhất)  
- **Template:** Free tier  

#### Thiết lập
- DB instance identifier: `target-postgres-db`  
- Master username: `postgres`  
- Password: `MyPassword123!`  

![Create Database Security Group](/images/1-env-setup/cr-db6.png)

#### Cấu hình Instance
- DB instance class: `db.t3.micro`

#### Lưu trữ
- Storage type: General Purpose SSD (gp2)  
- Allocated storage: 20 GiB  

#### Kết nối mạng
- VPC: `migration-vpc`  
- DB subnet group: `migration-db-subnet-group`  
- Public access: No  
- Security group: `db-migration-sg`  

![Create Database Security Group](/images/1-env-setup/cr-db7.png)

#### Cấu hình bổ sung
- Initial database name: `targetdb`  

![Create Database Security Group](/images/1-env-setup/cr-db8.png)

**Nhấn "Create database"** và chờ khoảng 10 phút.

📝 **Ghi chú:**
- Target DB Identifier: `target-postgres-db`
- Endpoint: *(có sau khi tạo xong)*
- Username: `postgres`
- Password: `MyPassword123!`
---
### 1.6 Tạo EC2
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
---
### 1.7 Kết nối EC2 và tạo dữ liệu mẫu
#### Kết nối tới EC2 và tạo dữ liệu mẫu

**Bước 1:** Kết nối tới EC2 Instance  
1. Chờ EC2 instance ở trạng thái **Running**.  
2. Sao chép **Public IPv4** address.  
3. Mở **cmd** hoặc **Powershell**.  
4. Di chuyển đến thư mục chứa file `migration-key.pem`:
```bash
cd C:\path\to\your\key\
```
Chạy lệnh kết nối SSH:

```bash
ssh -i migration-key.pem ec2-user@[Public IPV4]
```
**Bước 2:** Cài đặt MySQL Client

```bash
sudo yum update -y
sudo dnf install mariadb105
```
**Bước 3:** Tạo dữ liệu mẫu trong MySQL
Kết nối tới cơ sở dữ liệu MySQL:

```bash
mysql -h [SOURCE-MYSQL-ENDPOINT] -u admin -p
# Nhập mật khẩu: MyPassword123! 
```
![Create Database Security Group](/images/1-env-setup/connect-database.png)

**Bước 4:** Trong MySQL, tạo bảng và chèn dữ liệu mẫu:

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

-- Kiểm tra dữ liệu
SELECT COUNT(*) FROM employees;
SELECT COUNT(*) FROM departments;
```
Bước 5: Thoát MySQL:

```sql
EXIT;
```
![Create Database Security Group](/images/1-env-setup/complete-db.png)
---
### 1.8 Cấu hình DMS
---
title: "Tạo DMS Replication Instance và Endpoints"
date: 2025-08-09T00:00:00+07:00
weight: 8
chapter: false
pre: " <b> 1.8 </b> "
---

## Tạo DMS Replication Instance và Endpoints

### Tạo DMS Replication Instance

#### Truy cập DMS Console
- Tìm kiếm **"DMS"** trong thanh tìm kiếm của AWS Management Console.
- Chọn **Provisioned instances**.

#### Tạo Replication Instance
- Chọn **Create replication instance**.
![Create Database Security Group](/images/1-env-setup/dms-rev-ins.png)
- Cấu hình:
  - **Name:** `migration-replication-instance`
  - **Description:** `Replication instance for database migration`
  - **Instance class:** `dms.t3.micro`
  - **Engine version:** `(latest)`
  - **Multi-AZ:** `No` (để tiết kiệm chi phí)
![Create Database Security Group](/images/1-env-setup/cr-dms-rev-ins.png)

#### Kết nối và Bảo mật
- **VPC:** `migration-vpc`
- **Replication subnet group:** `migration-dms-subnet-group`
- **Publicly accessible:** `No`
- **VPC security group:** `dms-replication-sg`
![Create Database Security Group](/images/1-env-setup/connect-and-security.png)

Nhấn **Create replication instance** và chờ khoảng **10 phút** cho đến khi trạng thái là **Available**.

> 📝 **Ghi chú:**  
> Replication Instance: `migration-replication-instance`

---

### Tạo Source Endpoint

#### Truy cập Endpoints
- Trong DMS Console, chọn **Endpoints**.
- Nhấn **Create endpoint**.
![Create Database Security Group](/images/1-env-setup/cre-endp.png)

#### Cấu hình Endpoint
- **Endpoint type:** `Source endpoint`
- **Endpoint identifier:** `source-mysql-endpoint`
- **Source engine:** `MySQL`
![Create Database Security Group](/images/1-env-setup/src-endp.png)

#### Kết nối tới Source Database
- **Server name:** `[SOURCE-MYSQL-ENDPOINT]` (lấy từ RDS → Database → source-mysql-db → Connectivity & Security)
![Create Database Security Group](/images/1-env-setup/rds-endpoint.png)
- **Port:** `3306`
- **Username:** `admin`
- **Password:** `MyPassword123!`
![Create Database Security Group](/images/1-env-setup/src-endp2.png)

#### Kiểm tra kết nối Endpoint
- Chọn **Run test**.
- **VPC:** `migration-vpc`
- **Replication instance:** `migration-replication-instance`
- Nhấn **Run test** lần nữa và chờ cho đến khi trạng thái là **successful**.
![Create Database Security Group](/images/1-env-setup/test-endp.png)

#### Cài đặt nâng cao
- **Extra connection attributes:** *(để trống)*

Nhấn **Create endpoint**.

---

### Tạo Target Endpoint

#### Tạo Endpoint
- Nhấn **Create endpoint**.

#### Cấu hình Endpoint
- **Endpoint type:** `Target endpoint`
- **Endpoint identifier:** `target-postgres-endpoint`
- **Target engine:** `PostgreSQL`
![Create Database Security Group](/images/1-env-setup/targer-endp.png)

#### Kết nối tới Target Database
- **Server name:** `[TARGET-POSTGRES-ENDPOINT]`
- **Port:** `5432`
- **Username:** `postgres`
- **Password:** `MyPassword123!`
- **Database name:** `targetdb`
![Create Database Security Group](/images/1-env-setup/target-endp.png)

#### Kiểm tra kết nối Endpoint
- Chọn **Run test**.
- **Replication instance:** `migration-replication-instance`
- Nhấn **Run test** lần nữa và chờ cho đến khi trạng thái là **successful**.
- Nhấn **Create endpoint**.

> 📝 **Ghi chú:**  
> Nếu kiểm tra kết nối thất bại, vào **db-migration-sg** và thêm inbound rule: **PostgreSQL (5432)** → Source: (ID của **dms-replication-sg**)
