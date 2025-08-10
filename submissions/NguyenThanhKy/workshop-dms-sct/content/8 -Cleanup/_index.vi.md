---
title: "Dọn Dẹp Tài Nguyên"
date: 2025-08-09T00:00:00+07:00
weight: 8
chapter: false
pre: " <b> 8. </b> "
---

## Chương 9: Dọn Dẹp Tài Nguyên

### 9.1 Dừng các tài nguyên không cần thiết
1. **Dừng EC2 Instance:**  
   - Vào **EC2 Console** > **Instances**  
   - Chọn **migration-bastion**  
   - Tại **Instance State**, chọn **Stop instance**

2. **Dừng DMS Replication Instance:**  
   - Vào **DMS Console** > **Replication instances**  
   - Chọn **migration-replication-instance**  
   - Tại **Actions**, chọn **Stop**

---

### 9.2 Dọn dẹp toàn bộ (tuỳ chọn)
⚠️ **Cảnh báo:** Chỉ thực hiện bước này khi đã hoàn thành workshop.

```markdown
# Checklist Dọn Dẹp Toàn Bộ (Tuỳ chọn)
- [ ] Xoá các task migration DMS
- [ ] Xoá các endpoint DMS
- [ ] Xoá replication instance DMS
- [ ] Xoá các instance RDS
- [ ] Xoá các snapshot RDS
- [ ] Xoá các function Lambda
- [ ] Xoá các alarm CloudWatch
- [ ] Xoá dashboard CloudWatch
- [ ] Terminate các instance EC2
- [ ] Xoá VPC (bao gồm subnet, security group, v.v.)
