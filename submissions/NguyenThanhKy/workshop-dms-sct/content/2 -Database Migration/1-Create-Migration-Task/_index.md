---
title: "Create Migration Task"
date: 2025-08-09T00:00:00+07:00
weight: 1
chapter: false
pre: " <b> 2.1 </b> "
---

## Create Migration Task

### Create a Database Migration Task
- Go to **DMS Console** and select **Database migration tasks**
- Click **Create task**
![Create Database Security Group](/images/2-database-migration/create-task.png)

### Task configuration
- **Task identifier:** `mysql-to-postgres-migration`
- **Replication instance:** `migration-replication-instance`
- **Source database endpoint:** `source-mysql-endpoint`
- **Target database endpoint:** `target-postgres-endpoint`
- **Migration type:** *Migrate existing data and replicate data changes*
![Create Database Security Group](/images/2-database-migration/cr-task1.png)

### Task settings
- **Target table preparation mode:** Drop tables on target
- **Include LOB columns in replication:** Limited LOB mode
- **Max LOB size (KB):** 32
- **Enable validation:** Yes
- **Enable CloudWatch logs:** Yes
![Create Database Security Group](/images/2-database-migration/cr-task2.png)
### Table mappings
- **Editing mode:** Wizard
- Click **Add new selection rule** and configure:
  - **Schema:** sampledb
  - **Table name:** %
  - **Action:** Include
- Click **Create task**
![Create Database Security Group](/images/2-database-migration/cr-task-3.png)

