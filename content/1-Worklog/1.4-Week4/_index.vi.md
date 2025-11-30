---
title: "Worklog Tuần 4"
date: "2025-09-28"
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu Tuần 4

- Tìm hiểu sâu về containerization với Docker và những kiến thức cơ bản về ECS.
- Thiết lập VPC cơ bản với cấu hình networking thích hợp.
- Khám phá Elastic Load Balancer (ELB) cho phân phối traffic.

### Các công việc thực hiện trong tuần này
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|-----|------|------------|-----------------|--------------------|
| Thứ 2 | - Xem xét các khái niệm Docker và container images <br> - Học ECS task definitions và clusters | 18/09/2025 | 21/09/2025 | <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/">Tài liệu ECS</a> |
| Thứ 3 | - Weekly Scrum/Meeting: Báo cáo tiến độ và đồng bộ hóa với đội | 18/09/2025 | 21/09/2025 | — |
| Thứ 4 | - Thiết lập Application Load Balancer (ALB) <br> - Cấu hình target groups cho EC2 instances | 18/09/2025 | 21/09/2025 | <a href="https://docs.aws.amazon.com/elasticloadbalancing/">Tài liệu ELB</a> |
| Thứ 5 | - Khởi chạy nhiều EC2 instances trong VPC <br> - Đăng ký instances với load balancer | 18/09/2025 | 21/09/2025 | <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/">Hướng dẫn người dùng EC2</a> |
| Thứ 6 | - Kiểm tra load balancing trên các instances <br> - Theo dõi health checks và phân phối traffic | 18/09/2025 | 21/09/2025 | <a href="https://docs.aws.amazon.com/elasticloadbalancing/latest/application/">Hướng dẫn ALB</a> |
| Thứ 7 | - Tài liệu hóa kiến trúc VPC <br> - Tối ưu hóa security group rules | 18/09/2025 | 21/09/2025 | — |

### Những thành tích của Tuần 4

- Tạo thành công VPC với phân chia subnet thích hợp và lập kế hoạch CIDR.
- Cấu hình security groups với inbound và outbound rules theo nguyên tắc least-privilege.
- Thiết lập Application Load Balancer và đăng ký EC2 instances làm targets.
- Xác minh functionality của health checks và load balancing trên nhiều instances.
- Học những khác biệt chính giữa security groups và NACLs và các trường hợp sử dụng của chúng.
- Kiểm tra hành vi failover khi instances không healthy.

### Những thách thức và bài học

- CIDR block conflicts yêu cầu lập kế hoạch cẩn thận; overlapping subnets có thể gây ra routing issues.
- Cấu hình health check trực tiếp ảnh hưởng tới tính khả dụng của instance trong load balancer.
- Security group rules phải cho phép ALB traffic trên các port và protocol chính xác.
