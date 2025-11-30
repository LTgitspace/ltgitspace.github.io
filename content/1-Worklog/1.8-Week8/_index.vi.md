---
title: "Worklog Tuần 8"
date: "2025-10-26"
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu Tuần 8

- Thiết lập RDS cho managed relational database services.
- Triển khai basic backup và recovery strategies.
- Cấu hình security groups cho database access từ ECS services.

### Các công việc thực hiện trong tuần này
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|-----|------|------------|-----------------|--------------------|
| Thứ 2 | - Tạo RDS database instance (PostgreSQL/MySQL) <br> - Cấu hình multi-AZ deployment cho high availability | 16/10/2025 | 19/10/2025 | <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/">Tài liệu RDS</a> |
| Thứ 3 | - Weekly Scrum/Meeting: Báo cáo tiến độ và đồng bộ hóa với đội | 16/10/2025 | 19/10/2025 | — |
| Thứ 4 | - Tạo initial database schema và tables <br> - Thiết lập automated backups và retention policies | 16/10/2025 | 19/10/2025 | <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithBackups.html">Hướng dẫn RDS Backups</a> |
| Thứ 5 | - Bật Enhanced Monitoring cho RDS instance <br> - Cấu hình database parameter groups cho optimization | 16/10/2025 | 19/10/2025 | <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/MonitoringOverview.html">Theo dõi RDS</a> |
| Thứ 6 | - Cập nhật application connection strings trong ECS tasks <br> - Kiểm tra database operations từ containerized application | 16/10/2025 | 19/10/2025 | <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ConnectToInstance.html">Kết nối RDS</a> |
| Thứ 7 | - Thực hiện backup và restore test <br> - Tài liệu hóa database configuration và credentials management | 16/10/2025 | 19/10/2025 | — |

### Những thành tích của Tuần 8

- Cấp phát thành công multi-AZ RDS instance với automatic failover.
- Cấu hình proper security group ingress rules cho application access.
- Triển khai automated daily backups với 7-day retention.
- Xác minh application connectivity tới RDS database.
- Thiết lập Enhanced Monitoring để theo dõi database performance metrics.
- Học RDS parameter group customization và performance tuning basics.

### Những thách thức và bài học

- Database parameter changes có thể yêu cầu instance reboots; lập kế hoạch maintenance windows cẩn thận.
- Multi-AZ deployments gấp đôi chi phí; đánh giá nhu cầu trước khi bật.
- Application connection pooling là quan trọng để tránh exhausting available connections.
