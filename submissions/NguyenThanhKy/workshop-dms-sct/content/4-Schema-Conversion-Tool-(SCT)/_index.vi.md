---
title: "Công Cụ Chuyển Đổi Schema (SCT)"
date: 2025-08-09T00:00:00+07:00
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

**Nội dung:**
- [Tải & Cài đặt SCT](#tải--cài-đặt-sct)
- [Tạo Project SCT](#tạo-project-sct)
- [Phân tích & Chuyển đổi Schema](#phân-tích--chuyển-đổi-schema)

---

### Tải & Cài đặt SCT

#### Tải SCT
1. Truy cập: [AWS SCT](https://aws.amazon.com/dms/schema-conversion-tool/).  
2. Tải bản phù hợp với hệ điều hành của bạn.  
3. Cài đặt theo hướng dẫn.

#### Cài đặt JDBC Drivers
1. Tải **MySQL Connector/J**.  
2. Tải **PostgreSQL JDBC Driver**.  
3. Copy 2 file `.jar` này vào thư mục **drivers của SCT**.

---

### Tạo Project SCT

#### Tạo Project mới
1. Mở **AWS SCT**.  
2. Vào **File > New Project**.  
3. Tên project: `MySQL-to-PostgreSQL-Migration`  
4. Chọn thư mục lưu project.  
5. **Source database:** MySQL  
6. **Target database:** PostgreSQL  

![Kết nối SCT](/images/4-/sct1.png)

#### Kết nối tới Source Database (MySQL)
1. Chuột phải vào **MySQL** ở **panel source**.  
2. Chọn **Connect to MySQL**.  
3. Cấu hình:  
   - Server name: [SOURCE-MYSQL-ENDPOINT]  
   - Port: 3306  
   - User name: admin  
   - Password: MyPassword123!  
   - Database: sampledb  

> Nếu dùng localhost, chạy câu lệnh SSH: **ssh -i migration-key.pem -L 3306:[SOURCE-MYSQL-ENDPOINT]:3306 ec2-user@[EC2 IPv4]**


![Monitor Task Status](/images/4-/sct2.png)

4. Nhấn **Test Connection** → **OK** nếu thành công.

![Monitor Task Status](/images/4-/sct3.png)

#### Chọn schema

![Monitor Task Status](/images/4-/sct4.png)

#### Kết nối tới Target Database (PostgreSQL)
1. Chuột phải vào PostgreSQL ở panel target.

2. Chọn Connect to PostgreSQL.

3. Cấu hình:

Server name: [TARGET-POSTGRES-ENDPOINT]

Port: 5432

User name: postgres

Password: MyPassword123!

Database: targetdb

>Nếu dùng localhost, chạy câu lệnh SSH: **ssh -i migration-key.pem -L 5432:[TARGET-POSTGRES-ENDPOINT]:5432 ec2-user@[EC2 IPv4]**


![Monitor Task Status](/images/4-/sct5.png)

4. Nhấn **Test Connection** → **OK**.


![Monitor Task Status](/images/4-/sct6.png)

### Phân tích & Chuyển đổi Schema
#### Phân tích Schema nguồn
1. Mở rộng **sampledb** ở **source panel**.

2. Chuột phải **sampledb** → chọn **Create Report**.

Xem kết quả trong tab **Summary**.

![Monitor Task Status](/images/4-/sct7.png)

![Monitor Task Status](/images/4-/sct8.png)


#### Chuyển đổi Schema
1. Chuột phải **sampledb** → chọn **Convert Schema**.

2. Xem schema đã chuyển đổi ở **panel target**.

3. Kiểm tra **warnings/errors**(nếu có).

#### Áp dụng sang Target
1. Chuột phải schema đã chuyển đổi ở **target panel**.

2. Chọn **Apply to database**.

3. Xem lại các câu lệnh SQL.

4. Nhấn **Apply to database**.

![Monitor Task Status](/images/4-/sct9.png)