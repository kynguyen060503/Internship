---
title: "Tạo Security Groups"
date: 2025-08-09T00:00:00+07:00
weight: 2
chapter: false
pre: " <b> 1.2 </b> "
---

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
