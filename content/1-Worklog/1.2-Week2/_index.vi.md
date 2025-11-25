---
title: "Worklog Tuần 2"
date: "2025-09-21"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu Tuần 2

- Học các dịch vụ AWS được sử dụng phổ biến một cách sâu hơn.
- Tham gia một sự kiện AWS lớn và ghi lại những hiểu biết chính.

### Các công việc thực hiện trong tuần này
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|-----|------|------------|-----------------|--------------------|
| Thứ 2 | - Scrum đội để phát triển đề xuất dự án <br> - Thiết lập môi trường Linux | 11/09/2025 | 14/09/2025 | — |
| Thứ 3 | — | 11/09/2025 | 14/09/2025 | — |
| Thứ 4 | - Giúp các thành viên đội thiết lập môi trường Linux và Windows | 11/09/2025 | 14/09/2025 | <a href="https://ubuntu.com/tutorials">Hướng dẫn Ubuntu</a> · <a href="https://learn.microsoft.com/windows/wsl/install">Cài đặt WSL</a> |
| Thứ 5 | - Tham gia sự kiện AWS Cloud Day | 11/09/2025 | 14/09/2025 | <a href="https://aws.amazon.com/events/">Sự kiện AWS</a> |
| Thứ 6 | - Mua tên miền trên Route 53 và thiết lập SSL qua Cloudflare <br> - Đọc tài liệu cho EC2, ECS, ECR, EKS, VPC, Lambda | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html">Route 53: Đăng ký tên miền</a> · <a href="https://developers.cloudflare.com/ssl/">Cloudflare SSL/TLS</a> · <a href="https://docs.aws.amazon.com/ec2/">EC2</a> · <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html">ECS</a> · <a href="https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html">ECR</a> · <a href="https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html">EKS</a> · <a href="https://docs.aws.amazon.com/vpc/">VPC</a> · <a href="https://docs.aws.amazon.com/lambda/latest/dg/welcome.html">Lambda</a> |
| Thứ 7 | - Học EC2 Auto Scaling và triển khai một backend hiện có trên EC2 với Docker | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html">EC2 Auto Scaling</a> · <a href="https://docs.docker.com/engine/install/">Cài đặt Docker</a> |

### Những thành tích của Tuần 2

- Triển khai một ứng dụng web với một tên miền tùy chỉnh và HTTPS termination qua Cloudflare.
- Giúp các thành viên đội chuẩn hóa môi trường phát triển (Linux/Windows) để cộng tác mượt mà hơn.
- Tăng cường kiến thức trên các dịch vụ AWS chính: EC2, ECS, ECR, EKS, VPC, và Lambda.
- Thực hành các khái niệm scaling với EC2 Auto Scaling và triển khai containerized với Docker trên EC2.

### Những thách thức và bài học

- DNS và thay đổi chứng chỉ có thể mất thời gian để lan truyền; lên kế hoạch cho TTLs và xác thực các bản ghi trước khi cắt.
- Trộn lẫn các nhà cung cấp (Route 53 + Cloudflare) yêu cầu cấu hình name server và SSL/TLS cẩn thận.
- Thiết lập môi trường khác nhau giữa các hệ điều hành—ghi chép một đường dẫn thiết lập tối thiểu, có thể lặp lại cho các thành viên đội.
- Khi khắc phục sự cố triển khai trên EC2, hãy bắt đầu với security groups, NACLs, và logs của instance.
