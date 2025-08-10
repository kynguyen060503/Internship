---
title: "Performance Testing"
date: 2025-08-09T00:00:00+07:00
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

**Content:**
- [Create Performance Test Data](#create-performance-test-data)
- [Monitor Performance Metrics](#monitor-performance-metrics)

---

### Create Performance Test Data

#### 6.1.1 Connect to Source MySQL
```bash
mysql -h [SOURCE-MYSQL-ENDPOINT] -u admin -p sampledb
```
6.1.2 Create Large Dataset
```sql
-- Create a procedure to generate test data
DELIMITER //
DROP PROCEDURE IF EXISTS GenerateEmployees;
CREATE PROCEDURE GenerateEmployees(IN num_rows INT)
BEGIN
    DECLARE i INT DEFAULT 1;
    DECLARE start_id INT;

    -- Get current max ID
    SELECT IFNULL(MAX(id), 0) INTO start_id FROM employees;

    -- Start from next ID
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

-- Generate 1000 test records
CALL GenerateEmployees(20);

-- Verify
SELECT COUNT(*) FROM employees;
```


![Monitor Task Status](/images/5-/evb3.png)
### Monitor Performance Metrics
#### Check CloudWatch Metrics
1. Go back to the CloudWatch Dashboard.

2. Monitor these metrics:

- DMS throughput

- Replication lag

- RDS CPU and Memory usage

- Network I/O1

#### Performance Benchmark
1. Record baseline metrics.

2. Generate additional load.

3. Monitor the impact on migration performance.


![Monitor Task Status](/images/5-/evb4.png)