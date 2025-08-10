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
