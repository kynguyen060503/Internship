---
title: "Validate Migration"
date: 2025-08-09T00:00:00+07:00
weight: 3
chapter: false
pre: " <b> 2.3 </b> "
---

## Validate Migration

### Connect to Target PostgreSQL
1. Open your terminal
2. Connect EC2 agian like step 1.7

![Monitor Task Status](/images/2-database-migration/vo-ec2.png)

3. Install PostgreSQL client:
```bash
sudo yum install postgresql15 -y
```

3. Connect to PostgreSQL:
```bash
psql -h [TARGET-POSTGRES-ENDPOINT] -U postgres -d targetdb
Enter password: MyPassword123!
```

### Verify Migrated Data
- Run this line 
```sql
\dt
SELECT COUNT() FROM employees;
SELECT * FROM employees LIMIT 5;
SELECT COUNT() FROM departments;
SELECT * FROM departments;
\d employees
\d departments
```
- Result:
![Monitor Task Status](/images/2-database-migration/check-result-1.png)
![Monitor Task Status](/images/2-database-migration/check-result-2.png)
### Test CDC (Change Data Capture)
1. Open a new terminal and connect to MySQL:
```bash
mysql -h [SOURCE-MYSQL-ENDPOINT] -u admin -p sampledb
Enter password: MyPassword123!
```

2. Insert a new record:
```sql
INSERT INTO employees VALUES
(6, 'Alice', 'Green', 'alice.green@company.com', 'IT', 72000.00, '2024-01-10');
```
- Result:
![Monitor Task Status](/images/2-database-migration/check-result-3.png)
3. Go back to PostgreSQL and check:
```sql
SELECT COUNT(*) FROM employees;
SELECT * FROM employees WHERE id = 6;
```
- Result:
![Monitor Task Status](/images/2-database-migration/check-result-4.png)