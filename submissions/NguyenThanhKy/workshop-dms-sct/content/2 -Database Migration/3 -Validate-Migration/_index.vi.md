---
title: "Xác Thực Migration"
date: 2025-08-09T00:00:00+07:00
weight: 3
chapter: false
pre: " <b> 2.3 </b> "
---

## Xác Thực Migration

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