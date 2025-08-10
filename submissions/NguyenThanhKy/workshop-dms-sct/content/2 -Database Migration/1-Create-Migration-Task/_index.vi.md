---
title: "Tạo Migration Task"
date: 2025-08-09T00:00:00+07:00
weight: 1
chapter: false
pre: " <b> 2.1 </b> "
---

## Tạo Migration Task

### Tạo một Database Migration Task
- Vào **DMS Console** và chọn **Database migration tasks**  
- Nhấn **Create task**  

![Tạo Database Migration Task](/images/2-database-migration/create-task.png)

### Cấu hình Task
- **Task identifier:** `mysql-to-postgres-migration`  
- **Replication instance:** `migration-replication-instance`  
- **Source database endpoint:** `source-mysql-endpoint`  
- **Target database endpoint:** `target-postgres-endpoint`  
- **Migration type:** *Migrate existing data and replicate data changes*  

![Cấu hình Task](/images/2-database-migration/cr-task1.png)

### Cài đặt Task
- **Target table preparation mode:** Drop tables on target (Xóa bảng trên target trước khi tạo mới)  
- **Include LOB columns in replication:** Limited LOB mode (Giới hạn kích thước LOB)  
- **Max LOB size (KB):** 32  
- **Enable validation:** Yes (Bật xác thực dữ liệu)  
- **Enable CloudWatch logs:** Yes (Bật log lên CloudWatch)  

![Cài đặt Task](/images/2-database-migration/cr-task2.png)

### Mapping bảng
- **Editing mode:** Wizard  
- Nhấn **Add new selection rule** và cấu hình:  
  - **Schema:** sampledb  
  - **Table name:** %  
  - **Action:** Include  
- Nhấn **Create task**  

![Mapping bảng](/images/2-database-migration/cr-task-3.png)
