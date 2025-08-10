---
title : "Tạo Migration Task"
date: 2025-08-09T00:00:00+07:00
weight : 2
chapter : false
pre : " <b> 2. </b> "
---
**Content:**
- [Tạo Migration Task](#21-tạo-migration-task)
- [Giám Sát Tiến Trình Migration](#22-giám-sát-tiến-trình-migration)
- [Xác Thực Migration](#23-xác-thực-migration)
---
### 2.1 Tạo Migration Task

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
---
### 2.2 Giám Sát Tiến Trình Migration

### Giám sát trạng thái Task
1. Trong **"Database migration tasks"** trên AWS DMS Console  
2. Nhấn vào tên task: **mysql-to-postgres-migration**  
3. Theo dõi các tab sau:  
   - **Overview:** Xem trạng thái và tiến trình của task migration  
   - **Table statistics:** Trạng thái chi tiết của từng bảng  
   - **CloudWatch metrics:** Chỉ số hiệu suất replication  
   - **CloudWatch logs:** Log chi tiết quá trình migration  

![Giám sát trạng thái Task](/images/2-database-migration/mornitor-task.png)

### Kiểm tra thống kê bảng
- **Full load:** Xác minh rằng các bảng đã được tải đầy đủ  
- **Ongoing replication:** Đảm bảo dữ liệu thay đổi theo thời gian thực đang được đồng bộ  
- **Validation:** Kiểm tra tính nhất quán dữ liệu giữa source và target  

![Thống kê bảng](/images/2-database-migration/tb-stat.png)
---
### 2.3 Xác Thực Migration

### Kết nối đến PostgreSQL (Target)
1. Mở terminal  
2. Kết nối lại vào EC2 giống bước **1.7**  

![Vào EC2](/images/2-database-migration/vo-ec2.png)

3. Cài đặt PostgreSQL client:
```bash
sudo yum install postgresql15 -y
```
Kết nối đến PostgreSQL:

```bash
psql -h [TARGET-POSTGRES-ENDPOINT] -U postgres -d targetdb
Enter password: MyPassword123!
```
### Kiểm tra dữ liệu đã migrate
Chạy các lệnh sau:

```sql
\dt
SELECT COUNT(*) FROM employees;
SELECT * FROM employees LIMIT 5;
SELECT COUNT(*) FROM departments;
SELECT * FROM departments;
\d employees
\d departments
```
Kết quả:
![Monitor Task Status](/images/2-database-migration/check-result-1.png)
![Monitor Task Status](/images/2-database-migration/check-result-2.png)
### Kiểm tra CDC (Change Data Capture)
1. Mở một terminal mới và kết nối đến MySQL:

```bash
mysql -h [SOURCE-MYSQL-ENDPOINT] -u admin -p sampledb
Enter password: MyPassword123!
```
2. Thêm một bản ghi mới:

```sql
INSERT INTO employees VALUES
(6, 'Alice', 'Green', 'alice.green@company.com', 'IT', 72000.00, '2024-01-10');
```
Kết quả:
![Monitor Task Status](/images/2-database-migration/check-result-3.png)
3. Quay lại PostgreSQL và kiểm tra:

```sql
SELECT COUNT(*) FROM employees;
SELECT * FROM employees WHERE id = 6;
```
Kết quả:
![Monitor Task Status](/images/2-database-migration/check-result-4.png) 

