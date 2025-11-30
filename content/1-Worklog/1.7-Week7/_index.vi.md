---
title: "Worklog Tuần 7"
date: "2025-10-19"
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu Tuần 7

- Thiết lập Route53 cho DNS management và domain configuration.
- Cấu hình SSL/TLS certificates sử dụng AWS Certificate Manager.
- Triển khai HTTPS traffic routing thông qua load balancer.

### Các công việc thực hiện trong tuần này
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|-----|------|------------|-----------------|--------------------|
| Thứ 2 | - Đăng ký domain trên Route53 <br> - Cấu hình Route53 hosted zone | 09/10/2025 | 12/10/2025 | <a href="https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/">Tài liệu Route53</a> |
| Thứ 3 | - Weekly Scrum/Meeting: Báo cáo tiến độ và đồng bộ hóa với đội | 09/10/2025 | 12/10/2025 | — |
| Thứ 4 | - Tạo Route53 A records trỏ tới Application Load Balancer <br> - Kiểm tra DNS resolution | 09/10/2025 | 12/10/2025 | <a href="https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.html">DNS Record Types</a> |
| Thứ 5 | - Cấu hình HTTPS listener trên ALB với ACM certificate <br> - Thiết lập HTTP to HTTPS redirect | 09/10/2025 | 12/10/2025 | <a href="https://docs.aws.amazon.com/elasticloadbalancing/latest/application/listener-update-rules.html">ALB Listener Rules</a> |
| Thứ 6 | - Kiểm tra HTTPS connectivity và certificate validity <br> - Cấu hình DNS failover (tùy chọn) | 09/10/2025 | 12/10/2025 | <a href="https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover.html">DNS Failover Guide</a> |
| Thứ 7 | - Theo dõi Route53 query metrics <br> - Tài liệu hóa domain và certificate details | 09/10/2025 | 12/10/2025 | — |

### Những thành tích của Tuần 7

- Đăng ký thành công và cấu hình custom domain trên Route53.
- Lấy và xác thực SSL/TLS certificate qua ACM.
- Cấu hình HTTPS traffic trên ALB với proper certificate binding.
- Triển khai HTTP to HTTPS redirect cho security compliance.
- Kiểm tra DNS resolution và HTTPS connectivity từ nhiều vị trí.
- Học Route53 routing policies (simple, weighted, latency-based).

### Những thách thức và bài học

- DNS propagation tốn thời gian; cài đặt TTL ảnh hưởng tới tốc độ thay đổi có hiệu lực.
- ACM certificate validation yêu cầu DNS hoặc email verification.
- ALB listener rules phải properly match hostnames cho certificate validation.
