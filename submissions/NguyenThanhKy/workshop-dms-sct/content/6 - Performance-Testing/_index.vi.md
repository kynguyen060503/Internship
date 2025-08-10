---
title: "Kiểm Tra Hiệu Năng"
date: 2025-08-09T00:00:00+07:00
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

**Nội dung:**
- [Tạo dữ liệu kiểm tra hiệu năng](#tao-du-lieu-kiem-tra-hieu-nang)
- [Giám sát các chỉ số hiệu năng](#giam-sat-cac-chi-so-hieu-nang)

---

### Tạo dữ liệu kiểm tra hiệu năng

#### 6.1.1 Kết nối đến MySQL nguồn
```bash
mysql -h [SOURCE-MYSQL-ENDPOINT] -u admin -p sampledb
```
6.1.2 Tạo bộ dữ liệu lớn
```sql
-- Tạo stored procedure để sinh dữ liệu kiểm tra
DELIMITER //
DROP PROCEDURE IF EXISTS GenerateEmployees;
CREATE PROCEDURE GenerateEmployees(IN num_rows INT)
BEGIN
    DECLARE i INT DEFAULT 1;
    DECLARE start_id INT;

    -- Lấy ID lớn nhất hiện tại
    SELECT IFNULL(MAX(id), 0) INTO start_id FROM employees;

    -- Bắt đầu từ ID tiếp theo
    SET i = start_id + 1;

    WHILE i < start_id + 1 + num_rows DO
        INSERT INTO employees VALUES
        (i,
         CONCAT('First', i),
         CONCAT('Last', i),
         CONCAT('user', i, '@company.com'),
         CASE (i % 4)
           WHEN 0 THEN 'IT'
           WHEN 1 THEN 'HR'
           WHEN 2 THEN 'Finance'
           ELSE 'Marketing'
         END,
         50000 + (i % 50000),
         DATE_ADD('2020-01-01', INTERVAL (i % 1000) DAY)
        );
        SET i = i + 1;
    END WHILE;
END //
DELIMITER ;

-- Sinh 20 bản ghi kiểm tra
CALL GenerateEmployees(20);

-- Kiểm tra số bản ghi
SELECT COUNT(*) FROM employees;

```
![Monitor Task Status](/images/5-/evb3.png)
### Giám sát các chỉ số hiệu năng
#### Kiểm tra các Metrics trên CloudWatch
1. Quay lại Dashboard CloudWatch.

2. Giám sát các chỉ số sau:

- Throughput của DMS

- Replication lag

- CPU và bộ nhớ của RDS

- Network I/O

#### Đánh giá hiệu năng
Ghi lại các chỉ số cơ bản (baseline).

Tăng tải thêm dữ liệu.

Giám sát ảnh hưởng đến hiệu năng migration.


![Monitor Task Status](/images/5-/evb4.png)
