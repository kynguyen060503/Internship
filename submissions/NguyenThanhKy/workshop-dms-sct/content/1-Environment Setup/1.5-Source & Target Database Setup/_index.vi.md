---
title: "Cài đặt Cơ sở dữ liệu Nguồn & Đích"
date: 2025-08-09T00:00:00+07:00
weight: 5
chapter: false
pre: " <b> 1.5 </b> "
---

Trong phần này, bạn sẽ tạo **Cơ sở dữ liệu nguồn (MySQL)** và **Cơ sở dữ liệu đích (PostgreSQL)** trên Amazon RDS.

---

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
