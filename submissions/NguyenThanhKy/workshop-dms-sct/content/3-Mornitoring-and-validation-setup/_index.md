---
title: "Monitoring and Validation Setup"
date: 2025-08-09T00:00:00+07:00
weight: 3
chapter: false
pre: " <b> 3 </b> "
---

**Content:**
- [Setup CloudWatch Dashboard](#setup-cloudwatch-dashboard)
- [Setup CloudWatch Alarms](#setup-cloudwatch-alarms)

---

### Setup CloudWatch Dashboard

#### Access CloudWatch Console
1. In **AWS Console**, search for **CloudWatch**.
2. Click **Dashboards**.

#### Create a Custom Dashboard
1. Click **Create dashboard**.
2. **Dashboard name:** `Migration-Monitoring`.
3. Click **Create dashboard**.

![Monitor Task Status](/images/3-/cloudwatch-create.png)

#### Add DMS Metrics
1. Click **Add widget**.
2. Select **Line** chart.
3. Click **Configure**.
4. **Metrics configuration:**
   - Namespace: `AWS/DMS`
   - Metric name: `CDCLatencySource`
   - ReplicationInstanceIdentifier: `migration-replication-instance`

   ![Monitor Task Status](/images/3-/cw-2.png)
5. Click **Create widget**.

**Add additional widgets:**
- `CDCLatencyTarget`
- `FullLoadThroughputRowsSource`
- `FullLoadThroughputRowsTarget`

#### Add RDS Metrics
- Create a new widget.
- Namespace: `AWS/RDS`
- Metrics:
  - `CPUUtilization` for both databases
  - `DatabaseConnections`
  - `ReadLatency`, `WriteLatency`

Click **Save dashboard**.

![Monitor Task Status](/images/3-/cw-3.png)
---

### Setup CloudWatch Alarms

#### Create DMS Replication Lag Alarm
1. Go to **Alarms** → **All alarms**.
2. Click **Create alarm**.
![Monitor Task Status](/images/3-/cw-alarm.png)
3. Click **Select metric**.
4. Metric selection:
   - `AWS/DMS > ReplicationInstanceIdentifier`
   - Select `CDCLatencySource`
   - ReplicationInstanceIdentifier: `migration-replication-instance`

   ![Monitor Task Status](/images/3-/cw-al1.png)
   ![Monitor Task Status](/images/3-/cw-al2.png)
   ![Monitor Task Status](/images/3-/cw-al3.png)
   

**Conditions:**
- Threshold type: `Static`
- Whenever `CDCLatencySource` is: `Greater than 300` (5 minutes)
![Monitor Task Status](/images/3-/cw-al4.png)
**Actions:**
- Alarm state trigger: `In alarm`
- Send notification to: Create new topic
- Topic name: `migration-alerts`
- Email endpoint: `your-email@domain.com`
![Monitor Task Status](/images/3-/cw-al5.png)
Alarm name: `DMS-High-Replication-Lag`  
![Monitor Task Status](/images/3-/cw-al6.png)
Click **Create alarm**.
![Monitor Task Status](/images/3-/cw-al7.png)
---

#### Create RDS CPU Alarm
- Create a new alarm for **RDS CPU > 80%**.
- Metric: `AWS/RDS > CPUUtilization`
- DBInstanceIdentifier: `source-mysql-db`
- Threshold: `80%`
- Action: Use the same SNS topic `migration-alerts`.
