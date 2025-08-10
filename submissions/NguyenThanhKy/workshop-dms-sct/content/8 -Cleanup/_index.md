---
title: "Resource Cleanup"
date: 2025-08-09T00:00:00+07:00
weight: 8
chapter: false
pre: " <b> 8. </b> "
---

## Chapter 9: Resource Cleanup

### 9.1 Stop Non-Essential Resources
1. **Stop EC2 Instance:**
   - Go to **EC2 Console** > **Instances**
   - Select **migration-bastion**
   - From **Instance State**, choose **Stop instance**

2. **Stop DMS Replication Instance:**
   - Go to **DMS Console** > **Replication instances**
   - Select **migration-replication-instance**
   - From **Actions**, choose **Stop**

---

### 9.2 Optional Full Cleanup
⚠️ **Warning:** Only perform this step after completing the workshop.

```markdown
# Full Cleanup Checklist (Optional)
- [ ] Delete DMS migration tasks
- [ ] Delete DMS endpoints  
- [ ] Delete DMS replication instance
- [ ] Delete RDS instances
- [ ] Delete RDS snapshots
- [ ] Delete Lambda functions
- [ ] Delete CloudWatch alarms
- [ ] Delete CloudWatch dashboard
- [ ] Terminate EC2 instances
- [ ] Delete VPC (will also delete subnets, security groups, etc.)
```