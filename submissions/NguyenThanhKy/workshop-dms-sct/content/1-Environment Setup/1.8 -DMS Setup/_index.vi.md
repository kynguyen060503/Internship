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
