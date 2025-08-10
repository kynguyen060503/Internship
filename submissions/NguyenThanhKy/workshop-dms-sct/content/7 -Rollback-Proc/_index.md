---
title: "Rollback Procedures"
date: 2025-08-09T00:00:00+07:00
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

**Content:**
- [Create Rollback Plan](#create-rollback-plan)
- [Execute Rollback Test](#execute-rollback-test)

---

### Create Rollback Plan

#### Stop Migration Task
1. In the **DMS Console** > **"Database migration tasks"**.  
2. Select the task: **mysql-to-postgres-migration**.  
3. Click **"Actions"** > **"Stop"**.  
4. Confirm stop.  

#### Create RDS Snapshots
1. In the **RDS Console** > **"Databases"**.  
2. Select **source-mysql-db**.  
3. Click **"Actions"** > **"Take snapshot"**.  
4. **Snapshot identifier:** `mysql-pre-rollback-snapshot`.  
5. Click **"Take snapshot"**.  

6. Repeat for **target-postgres-db**:
   - **Snapshot identifier:** `postgres-pre-rollback-snapshot`


![Monitor Task Status](/images/7-/snapshort.png)
---

### Execute Rollback Test

#### Simulate Rollback Scenario
1. Connect to the target PostgreSQL database:
```bash
psql -h [TARGET-POSTGRES-ENDPOINT] -U postgres -d targetdb
```
2. Delete some data to simulate corruption:

```sql
DELETE FROM employees WHERE id > 10;
SELECT COUNT(*) FROM employees;
```

![Monitor Task Status](/images/7-/res_demo.png)
#### Restore from Snapshot
1. In the RDS Console > "Snapshots".

2. Select postgres-pre-rollback-snapshot.

3. Click "Actions" > "Restore snapshot".

![Monitor Task Status](/images/7-/res-snap.png)

Configure:

DB instance identifier: target-postgres-db-restored
DB instance class: db.t3.micro
VPC: migration-vpc
DB subnet group: migration-db-subnet-group
Click "Restore DB instance".

![Monitor Task Status](/images/7-/res.png)

#### Verify Rollback
1. Wait for the restored instance to become Available (~10 minutes).

2. Connect to the restored instance.

3. Verify data integrity:

```sql
SELECT COUNT(*) FROM employees;
SELECT * FROM employees WHERE id > 1000 LIMIT 5;
```

![Monitor Task Status](/images/7-/snap-ré.png)