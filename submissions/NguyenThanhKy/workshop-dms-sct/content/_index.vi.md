---
title: "Environment Setup"
date: 2025-08-09T00:00:00+07:00
weight: 1
chapter: false
pre: " <b> </b> "
---

# Xây dựng Nền tảng Tự động Di chuyển Cơ sở dữ liệu với DMS & SCT

## Tổng quan
Trong workshop toàn diện này, bạn sẽ học cách xây dựng một nền tảng **Tự động di chuyển cơ sở dữ liệu** sử dụng **AWS Database Migration Service (DMS)** và **Schema Conversion Tool (SCT)**.  
Bạn sẽ tạo ra một **Migration Factory** hoàn chỉnh, hỗ trợ nhiều loại hệ quản trị cơ sở dữ liệu, triển khai di chuyển **zero-downtime** với **Change Data Capture (CDC)**, đồng thời bao gồm quy trình kiểm tra, xác thực và rollback tự động.  

Kết thúc workshop, bạn sẽ có kinh nghiệm thực hành các kỹ thuật và quy trình di chuyển cơ sở dữ liệu ở cấp độ doanh nghiệp, được áp dụng thực tế trong môi trường sản xuất.

![Kiến trúc AWS](/images/aws-achitecture.jpg)

---

## AWS Database Migration Service (DMS)
AWS DMS là dịch vụ được quản lý giúp bạn di chuyển cơ sở dữ liệu lên AWS nhanh chóng và an toàn.  
DMS hỗ trợ cả **di chuyển đồng nhất** (cùng loại cơ sở dữ liệu) và **di chuyển không đồng nhất** (khác loại cơ sở dữ liệu) như Oracle sang Amazon Aurora hoặc Microsoft SQL Server sang MySQL.  
Dịch vụ này giữ cho cơ sở dữ liệu nguồn vẫn hoạt động trong quá trình di chuyển, giúp giảm thiểu tối đa thời gian gián đoạn cho các ứng dụng đang sử dụng cơ sở dữ liệu đó.

{{% notice info %}}
DMS có thể di chuyển dữ liệu của bạn đến và từ hầu hết các cơ sở dữ liệu thương mại và mã nguồn mở phổ biến.  
Nó hỗ trợ sao chép dữ liệu liên tục với tính khả dụng cao và hợp nhất dữ liệu vào kho dữ liệu ở quy mô petabyte bằng cách stream dữ liệu sang Amazon Redshift và Amazon S3.
{{% /notice %}}

---

## Schema Conversion Tool (SCT)
AWS SCT giúp cho việc di chuyển cơ sở dữ liệu không đồng nhất trở nên dễ dàng hơn bằng cách tự động chuyển đổi cấu trúc (schema) và phần lớn mã đối tượng của cơ sở dữ liệu nguồn sang định dạng tương thích với cơ sở dữ liệu đích.  
Các đối tượng không thể chuyển đổi tự động sẽ được đánh dấu rõ ràng để bạn xử lý thủ công, đảm bảo quá trình di chuyển hoàn tất.

---

## Các thành phần của Database Migration
Một **Migration Factory** bao gồm một số thành phần chính phối hợp với nhau để đảm bảo di chuyển cơ sở dữ liệu thành công:

- **Replication Instance**: Tài nguyên tính toán thực hiện quá trình di chuyển dữ liệu  
- **Source và Target Endpoints**: Cấu hình kết nối tới cơ sở dữ liệu nguồn và đích  
- **Migration Tasks**: Xác định dữ liệu cần di chuyển và cách thức di chuyển  
- **Giám sát và Xác thực**: Theo dõi tiến trình di chuyển theo thời gian thực và đảm bảo tính toàn vẹn dữ liệu  

---

## Di chuyển Zero-Downtime
Di chuyển **zero-downtime** được thực hiện bằng cách kết hợp **full load migration** (tải toàn bộ dữ liệu ban đầu) với **Change Data Capture (CDC)**.  
Cách tiếp cận này đảm bảo ứng dụng của bạn vẫn có thể truy cập cơ sở dữ liệu nguồn trong khi các thay đổi được đồng bộ liên tục sang cơ sở dữ liệu đích gần như theo thời gian thực.

{{% notice tip %}}
Workshop sử dụng phương pháp tối ưu chi phí với các dịch vụ thuộc AWS Free Tier khi có thể.  
Tổng chi phí ước tính dưới $50, rất phù hợp cho mục đích học tập và phát triển.
{{% /notice %}}

---

## Thực hành tốt nhất (Best Practices) khi di chuyển
Workshop này áp dụng các thực hành tốt nhất trong di chuyển cơ sở dữ liệu bao gồm:

- Đánh giá và lập kế hoạch tiền di chuyển  
- Tự động chuyển đổi schema và xác thực  
- Kiểm thử hiệu năng và tối ưu hóa  
- Quy trình rollback và khôi phục sau sự cố  
- Chiến lược tối ưu chi phí khi triển khai sản xuất  

---

## Kiến trúc Workshop
Nền tảng di chuyển mà bạn sẽ xây dựng bao gồm:

- **Hỗ trợ nhiều loại cơ sở dữ liệu**: Ví dụ MySQL sang PostgreSQL  
- **Xác thực tự động**: Lambda functions kiểm tra tính toàn vẹn dữ liệu  
- **Giám sát thời gian thực**: Dashboard và cảnh báo trong CloudWatch  
- **Khả năng rollback**: Khôi phục dựa trên snapshot  
- **Tối ưu chi phí**: Lên lịch tắt/bật tài nguyên và quy trình dọn dẹp  

---
