---
title: "Tạo DB Subnet Group"
date: 2025-08-09T00:00:00+07:00
weight: 3
chapter: false
pre: " <b> 1.3 </b> "
---

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
