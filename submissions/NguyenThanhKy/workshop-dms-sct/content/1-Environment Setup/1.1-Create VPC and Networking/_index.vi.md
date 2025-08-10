---
title: "Tạo VPC và Cấu hình Mạng"
date: 2025-08-09T00:00:00+07:00
weight: 1
chapter: false
pre: " <b> 1.1 </b> "
---

#### Truy cập VPC Console
1. Đăng nhập vào **AWS Management Console**.

![VPC Console](/images/1-env-setup/aws-console.png)

2. Tìm kiếm **VPC** trong thanh tìm kiếm.

![Find VPC](/images/1-env-setup/find-vpc.png)

3. Nhấn chọn dịch vụ **VPC**.

---

#### Tạo VPC mới
1. Chọn **Create VPC** → chọn **VPC and more**.

![Create VPC](/images/1-env-setup/create-vpc.png)

2. Cấu hình:
   - **Name tag:** `migration-vpc`
   - **IPv4 CIDR:** `10.0.0.0/16`
   - **Number of AZs:** `2`
   - **Public subnets:** `2`
   - **Private subnets:** `2`
   - **NAT gateways:** `In 1 AZ`
   - **VPC endpoints:** `None`
   
![Create VPC1](/images/1-env-setup/create-vpc1.png)

![Create VPC2](/images/1-env-setup/create-vpc2.png)

3. Nhấn **Create VPC** và chờ khoảng 5 phút để AWS khởi tạo.

![Create VPC3](/images/1-env-setup/create-vpc3.png)

---

#### Ghi chú quan trọng
Sau khi tạo xong, lưu lại các thông tin sau để sử dụng trong các bước tiếp theo:

- **VPC ID:** `vpc-xxxxxxxxx` *(ID VPC vừa tạo)*
- **Public Subnet 1 ID:** `subnet-xxxxxxxxx`
- **Public Subnet 2 ID:** `subnet-xxxxxxxxx`
- **Private Subnet 1 ID:** `subnet-xxxxxxxxx`
- **Private Subnet 2 ID:** `subnet-xxxxxxxxx`

> 💡 **Tip:** Bạn có thể tìm các ID này trong trang **Subnets** và **VPCs** của AWS Console.
