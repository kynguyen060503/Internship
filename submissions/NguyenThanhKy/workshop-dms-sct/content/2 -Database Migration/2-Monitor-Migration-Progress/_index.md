---
title: "Monitor Migration Progress"
date: 2025-08-09T00:00:00+07:00
weight: 2
chapter: false
pre: " <b> 2.2 </b> "
---

## Monitor Migration Progress

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
