---
title: "Worklog"
date: "2025-01-01"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

## Thực Tập AWS Cloud 12 Tuần

Trong 12 tuần, tôi đã học và triển khai các dịch vụ AWS cơ bản, với 6 tuần đầu tập trung vào nền tảng và 6 tuần còn lại xây dựng Nền Tảng Hỗ Trợ Bỏ Thuốc Lá thực tế.

### Tổng Quan Hàng Tuần:

**[Tuần 1: Nền Tảng AWS](1.1-week1/)** - Giới thiệu AWS, học EC2, S3, và hoạt động CLI cơ bản, thiết lập tài khoản Free Tier, triển khai website tĩnh đầu tiên trên S3.

**[Tuần 2: Dịch Vụ Cơ Bản & Thiết Lập](1.2-week2/)** - Khám phá EC2, ECS, ECR, VPC, Lambda, Route53, và cơ bản Docker, thiết lập môi trường phát triển, đăng ký tên miền.

**[Tuần 3: Tích Hợp Frontend & Backend](1.3-week3/)** - Xây dựng ứng dụng Angular, tích hợp xác thực Cognito, lưu trữ tệp S3, và Lambda API endpoints với logging.

**[Tuần 4: Nền Tảng Networking](1.4-week4/)** - Tạo VPC với subnets, cấu hình security groups, thiết lập Application Load Balancer, kiểm tra phân phối traffic.

**[Tuần 5: Triển Khai Container](1.5-week5/)** - Xây dựng Docker images, push lên ECR, triển khai với ECS Fargate, cấu hình auto-scaling.

**[Tuần 6: CI/CD Pipeline](1.6-week6/)** - Thiết lập CodePipeline với GitHub integration, automated Docker builds với CodeBuild, triển khai deployment stages.

**[Tuần 7: Route53 DNS & HTTPS](1.7-week7/)** - Cấu hình custom domain trên Route53, lấy SSL/TLS certificates qua ACM, bảo mật ứng dụng với HTTPS trên ALB.

**[Tuần 8: Thiết Lập RDS Database](1.8-week8/)** - Tạo RDS instance để lưu trữ dữ liệu người dùng, cấu hình multi-AZ cho độ tin cậy, thiết lập backups và security groups.

**[Tuần 9: NoSQL & Session Storage](1.9-week9/)** - Xây dựng DynamoDB tables cho session management và tracking tiến độ người dùng, cấu hình indexes cho queries hiệu quả.

**[Tuần 10: S3 Storage & Optimization](1.10-week10/)** - Thiết lập S3 cho lưu trữ achievement badges và tài liệu người dùng, triển khai lifecycle policies và encryption cho bảo mật.

**[Tuần 11: Monitoring & Notifications](1.11-week11/)** - Tạo CloudWatch dashboards cho health của nền tảng, thiết lập alarms cho các metrics quan trọng, triển khai SNS cho notifications người dùng.

**[Tuần 12: Testing & Deployment](1.12-week12/)** - Kiểm tra end-to-end các tính năng bỏ thuốc, thực hiện load testing, triển khai nền tảng production-ready.

### Những Bài Học Chính

- Dịch vụ AWS cơ bản: EC2, S3, RDS, DynamoDB, VPC, IAM, Lambda
- Containerization với Docker và ECS
- Khái niệm Infrastructure as code với CloudFormation basics
- Best practices cho monitoring và logging
- Các chiến lược tối ưu hóa chi phí
