---
title: "Kết nối và tạo dữ liệu mẫu"
date: 2025-08-09T00:00:00+07:00
weight: 7
chapter: false
pre: " <b> 1.7 </b> "
---

#### Kết nối tới EC2 và tạo dữ liệu mẫu

**Bước 1:** Kết nối tới EC2 Instance  
1. Chờ EC2 instance ở trạng thái **Running**.  
2. Sao chép **Public IPv4** address.  
3. Mở **cmd** hoặc **Powershell**.  
4. Di chuyển đến thư mục chứa file `migration-key.pem`:
```bash
cd C:\path\to\your\key\
```
Chạy lệnh kết nối SSH:

```bash
ssh -i migration-key.pem ec2-user@[Public IPV4]
```
**Bước 2:** Cài đặt MySQL Client

```bash
sudo yum update -y
sudo dnf install mariadb105
```
**Bước 3:** Tạo dữ liệu mẫu trong MySQL
Kết nối tới cơ sở dữ liệu MySQL:

```bash
mysql -h [SOURCE-MYSQL-ENDPOINT] -u admin -p
# Nhập mật khẩu: MyPassword123! 
```
![Create Database Security Group](/images/1-env-setup/connect-database.png)

**Bước 4:** Trong MySQL, tạo bảng và chèn dữ liệu mẫu:

```sql
USE sampledb;

CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    hire_date DATE
);

INSERT INTO employees VALUES
(1, 'John', 'Doe', 'john.doe@company.com', 'IT', 75000.00, '2022-01-15'),
(2, 'Jane', 'Smith', 'jane.smith@company.com', 'HR', 65000.00, '2022-02-20'),
(3, 'Mike', 'Johnson', 'mike.johnson@company.com', 'Finance', 80000.00, '2022-03-10'),
(4, 'Sarah', 'Wilson', 'sarah.wilson@company.com', 'IT', 78000.00, '2022-04-05'),
(5, 'David', 'Brown', 'david.brown@company.com', 'Marketing', 70000.00, '2022-05-12');

CREATE TABLE departments (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    manager_id INT,
    budget DECIMAL(12,2)
);

INSERT INTO departments VALUES
(1, 'IT', 1, 500000.00),
(2, 'HR', 2, 300000.00),
(3, 'Finance', 3, 400000.00),
(4, 'Marketing', 5, 350000.00);

-- Kiểm tra dữ liệu
SELECT COUNT(*) FROM employees;
SELECT COUNT(*) FROM departments;
```
Bước 5: Thoát MySQL:

```sql
EXIT;
```
![Create Database Security Group](/images/1-env-setup/complete-db.png)