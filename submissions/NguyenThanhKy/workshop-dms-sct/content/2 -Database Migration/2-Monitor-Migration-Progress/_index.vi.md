---
title: "Giám Sát Tiến Trình Migration"
date: 2025-08-09T00:00:00+07:00
weight: 2
chapter: false
pre: " <b> 2.2 </b> "
---

## Giám Sát Tiến Trình Migration

### Giám sát trạng thái Task
1. Trong **"Database migration tasks"** trên AWS DMS Console  
2. Nhấn vào tên task: **mysql-to-postgres-migration**  
3. Theo dõi các tab sau:  
   - **Overview:** Xem trạng thái và tiến trình của task migration  
   - **Table statistics:** Trạng thái chi tiết của từng bảng  
   - **CloudWatch metrics:** Chỉ số hiệu suất replication  
   - **CloudWatch logs:** Log chi tiết quá trình migration  

![Giám sát trạng thái Task](/images/2-database-migration/mornitor-task.png)

### Kiểm tra thống kê bảng
- **Full load:** Xác minh rằng các bảng đã được tải đầy đủ  
- **Ongoing replication:** Đảm bảo dữ liệu thay đổi theo thời gian thực đang được đồng bộ  
- **Validation:** Kiểm tra tính nhất quán dữ liệu giữa source và target  

![Thống kê bảng](/images/2-database-migration/tb-stat.png)
