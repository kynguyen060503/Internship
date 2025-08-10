---
title: "Migration Task Setup & Validation"
date: 2025-08-09T00:00:00+07:00
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

**Content:**
- [Create Migration Task](#create-migration-task)
- [Monitor Migration Progress](#monitor-migration-progress)
- [Validate Migration](#validate-migration)

---

### Create Migration Task

#### 2.1.1 Access DMS Console
1. In **AWS Console**, search for **DMS**.  
2. Click **Database Migration Service**.  
3. Go to **Database migration tasks**.  
4. Click **Create task**.

#### 2.1.2 Configure Task
1. **Task identifier:** `mysql-to-postgres-migration`  
2. **Replication instance:** `migration-replication-instance`  
3. **Source endpoint:** `source-mysql-endpoint`  
4. **Target endpoint:** `target-postgres-endpoint`  
5. **Migration type:** `Migrate existing data and replicate data changes`

#### 2.1.3 Task Settings
- **Target table preparation mode:** Drop tables on target  
- **Include LOB columns in replication:** Limited LOB mode  
- **Max LOB size (KB):** 32  
- **Enable validation:** Yes  
- **Enable CloudWatch logs:** Yes

#### 2.1.4 Table Mappings
1. **Editing mode:** Wizard  
2. Click **Add new selection rule** and configure:  
   - **Schema:** `sampledb`  
   - **Table name:** `%`  
   - **Action:** Include  
3. Click **Create task**.

---

### Monitor Migration Progress

#### 2.2.1 Check Task Status
1. In **Database migration tasks**, click task name: `mysql-to-postgres-migration`.  
2. Monitor these tabs:  
   - **Overview:** Task status & progress  
   - **Table statistics:** Per-table details  
   - **CloudWatch metrics:** Performance metrics  
   - **CloudWatch logs:** Detailed logs

#### 2.2.2 Verify Table Statistics
- **Full load:** Tables loaded completely  
- **Ongoing replication:** Real-time data changes applied  
- **Validation:** Data consistency check

📝 **Progress Notes**  
- **Full load completed:** ✓  
- **CDC enabled:** ✓  
- **Validation status:** ✓  

---

### Validate Migration

#### Connect to Target PostgreSQL
1. From **EC2 Bastion Host**, install PostgreSQL client:  
   ```bash
   sudo yum install postgresql15 -y
   ```
2. Connect to PostgreSQL:

```bash
psql -h [TARGET-POSTGRES-ENDPOINT] -U postgres -d targetdb
Password: MyPassword123!
```
#### Verify Migrated Data
```sql
-- List all tables
\dt

-- Check employees table
SELECT COUNT(*) FROM employees;
SELECT * FROM employees LIMIT 5;

-- Check departments table
SELECT COUNT(*) FROM departments;
SELECT * FROM departments;

-- Verify data types
\d employees
\d departments
```
### Test CDC (Change Data Capture)
In a new terminal, connect to MySQL source:

```bash
mysql -h [SOURCE-MYSQL-ENDPOINT] -u admin -p sampledb
```
Insert new record:

```sql
INSERT INTO employees VALUES
(6, 'Alice', 'Green', 'alice.green@company.com', 'IT', 72000.00, '2024-01-10');
Back in PostgreSQL, verify:
```
```sql
SELECT COUNT(*) FROM employees;
SELECT * FROM employees WHERE id = 6;
```
---
### 2.2 Mornitor Migration Progress
### Monitor Task Status
1. In **"Database migration tasks"** on the AWS DMS Console
2. Click the task name: **mysql-to-postgres-migration**
3. Monitor the following tabs:
   - **Overview:** View migration task status and progress
   - **Table statistics:** Detailed status of each table
   - **CloudWatch metrics:** Replication performance metrics
   - **CloudWatch logs:** Detailed migration logs
![Monitor Task Status](/images/2-database-migration/mornitor-task.png)

### Check Table Statistics
- **Full load:** Verify that tables are completely loaded
- **Ongoing replication:** Ensure real-time data changes are being replicated
- **Validation:** Check data consistency between source and target
![Table Statistics](/images/2-database-migration/tb-stat.png)
---
### 2.3 Validate Migration 

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
###  Test CDC (Change Data Capture)
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