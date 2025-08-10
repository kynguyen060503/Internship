---
title: "Quy Trình Rollback"
date: 2025-08-09T00:00:00+07:00
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

**Nội dung:**
- [Tạo Kế Hoạch Rollback](#tao-ke-hoach-rollback)
- [Thực Hiện Kiểm Tra Rollback](#thuc-hien-kiem-tra-rollback)

---

### Tạo Kế Hoạch Rollback

#### Dừng Migration Task
1. Vào **DMS Console** > **"Database migration tasks"**.  
2. Chọn task: **mysql-to-postgres-migration**.  
3. Nhấn **"Actions"** > **"Stop"**.  
4. Xác nhận dừng task.

#### Tạo Snapshot RDS
1. Vào **RDS Console** > **"Databases"**.  
2. Chọn database **source-mysql-db**.  
3. Nhấn **"Actions"** > **"Take snapshot"**.  
4. Đặt tên snapshot: `mysql-pre-rollback-snapshot`.  
5. Nhấn **"Take snapshot"**.

6. Lặp lại cho **target-postgres-db**:  
   - Tên snapshot: `postgres-pre-rollback-snapshot`  

![Snapshot RDS](/images/7-/snapshort.png)

---

### Thực Hiện Kiểm Tra Rollback

#### Mô phỏng kịch bản Rollback
1. Kết nối đến PostgreSQL target:
```bash
psql -h [TARGET-POSTGRES-ENDPOINT] -U postgres -d targetdb
```
2. Xóa một số dữ liệu để giả lập lỗi:

```sql
Sao chép
Chỉnh sửa
DELETE FROM employees WHERE id > 10;
SELECT COUNT(*) FROM employees;
```
![Monitor Task Status](/images/7-/res_demo.png)

#### Khôi phục từ Snapshot
1. Vào RDS Console > Snapshots.

2. Chọn snapshot postgres-pre-rollback-snapshot.

3. Nhấn "Actions" > "Restore snapshot".

![Monitor Task Status](/images/7-/res-snap.png)

Cấu hình:

DB instance identifier: target-postgres-db-restored

DB instance class: db.t3.micro

VPC: migration-vpc

DB subnet group: migration-db-subnet-group
Nhấn "Restore DB instance".



#### Kiểm tra Rollback
1. Chờ instance mới khôi phục chuyển sang trạng thái Available (~10 phút).

2. Kết nối tới instance mới khôi phục.

3. Kiểm tra dữ liệu còn nguyên vẹn:

```sql
SELECT COUNT(*) FROM employees;
SELECT * FROM employees WHERE id > 1000 LIMIT 5;
```

![Monitor Task Status](/images/7-/snap-ré.png)