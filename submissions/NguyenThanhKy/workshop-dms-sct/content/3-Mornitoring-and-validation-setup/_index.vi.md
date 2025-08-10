---
title: "Cài Đặt Giám Sát và Xác Thực"
date: 2025-08-09T00:00:00+07:00
weight: 3
chapter: false
pre: " <b> 3 </b> "
---
**Content:**
- [Thiết lập CloudWatch Dashboard](#thiết-lập-cloudwatch-dashboard)
- [Thiết lập CloudWatch Alarms](#thiết-lập-cloudwatch-alarms)

---
### Thiết lập CloudWatch Dashboard

#### Truy cập CloudWatch Console
1. Trong **AWS Console**, tìm kiếm **CloudWatch**.  
2. Chọn **Dashboards**.  

#### Tạo Dashboard tùy chỉnh
1. Nhấn **Create dashboard**.  
2. **Dashboard name:** `Migration-Monitoring`  
3. Nhấn **Create dashboard**  

![Tạo Dashboard](/images/3-/cloudwatch-create.png)

#### Thêm DMS Metrics
1. Nhấn **Add widget**  
2. Chọn biểu đồ dạng **Line**  
3. Nhấn **Configure**  
4. **Cấu hình Metrics:**
   - Namespace: `AWS/DMS`
   - Metric name: `CDCLatencySource`
   - ReplicationInstanceIdentifier: `migration-replication-instance`  

   ![Cấu hình DMS Metrics](/images/3-/cw-2.png)
5. Nhấn **Create widget**  

**Thêm các widget khác:**
- `CDCLatencyTarget`
- `FullLoadThroughputRowsSource`
- `FullLoadThroughputRowsTarget`

#### Thêm RDS Metrics
- Tạo một widget mới.  
- Namespace: `AWS/RDS`  
- Metrics:  
  - `CPUUtilization` cho cả hai database  
  - `DatabaseConnections`  
  - `ReadLatency`, `WriteLatency`  

Nhấn **Save dashboard**  

![CloudWatch Dashboard](/images/3-/cw-3.png)

---

### Thiết lập CloudWatch Alarms

#### Tạo cảnh báo DMS Replication Lag
1. Vào **Alarms** → **All alarms**  
2. Nhấn **Create alarm**  
![Tạo Alarm](/images/3-/cw-alarm.png)  
3. Nhấn **Select metric**  
4. Chọn metric:
   - `AWS/DMS > ReplicationInstanceIdentifier`
   - Chọn `CDCLatencySource`
   - ReplicationInstanceIdentifier: `migration-replication-instance`  

   ![Chọn metric](/images/3-/cw-al1.png)  
   ![Chọn metric](/images/3-/cw-al2.png)  
   ![Chọn metric](/images/3-/cw-al3.png)  

**Điều kiện (Conditions):**
- Threshold type: `Static`
- Khi `CDCLatencySource`: `Greater than 300` (5 phút)  

![Cấu hình điều kiện](/images/3-/cw-al4.png)

**Hành động (Actions):**
- Alarm state trigger: `In alarm`
- Gửi thông báo đến: Tạo topic mới
- Tên topic: `migration-alerts`
- Email endpoint: `your-email@domain.com`  

![Cấu hình Actions](/images/3-/cw-al5.png)  

**Alarm name:** `DMS-High-Replication-Lag`  
![Tên Alarm](/images/3-/cw-al6.png)  

Nhấn **Create alarm**  
![Hoàn tất tạo alarm](/images/3-/cw-al7.png)

---

#### Tạo cảnh báo CPU cho RDS
- Tạo một alarm mới khi **RDS CPU > 80%**  
- Metric: `AWS/RDS > CPUUtilization`  
- DBInstanceIdentifier: `source-mysql-db`  
- Ngưỡng (Threshold): `80%`  
- Hành động: Sử dụng giống với `migration-alerts`  
