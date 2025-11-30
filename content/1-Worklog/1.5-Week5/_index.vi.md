---
title: "Worklog Tuần 5"
date: "2025-10-05"
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu Tuần 5

- Xây dựng và push Docker images lên Amazon ECR.
- Triển khai ứng dụng containerized sử dụng ECS Fargate.
- Tích hợp ECR với CodePipeline cho automated deployments.

### Các công việc thực hiện trong tuần này
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|-----|------|------------|-----------------|--------------------|
| Thứ 2 | - Tạo ECR repositories cho application images <br> - Cấu hình IAM permissions cho ECR access | 25/09/2025 | 28/09/2025 | <a href="https://docs.aws.amazon.com/AmazonECR/latest/userguide/">Tài liệu ECR</a> |
| Thứ 3 | - Weekly Scrum/Meeting: Báo cáo tiến độ và đồng bộ hóa với đội | 25/09/2025 | 28/09/2025 | — |
| Thứ 4 | - Tạo ECS Fargate task definitions <br> - Cấu hình container environment variables và resource limits | 25/09/2025 | 28/09/2025 | <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html">Hướng dẫn Task Definitions</a> |
| Thứ 5 | - Khởi chạy ECS services sử dụng Fargate launch type <br> - Gắn services tới load balancer được tạo ở Tuần 4 | 25/09/2025 | 28/09/2025 | <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html">Hướng dẫn ECS Services</a> |
| Thứ 6 | - Kiểm tra automatic task scaling dựa trên CPU và memory <br> - Xác minh service availability thông qua load balancer | 25/09/2025 | 28/09/2025 | <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-auto-scaling.html">Hướng dẫn Auto Scaling</a> |
| Thứ 7 | - Cập nhật image version trong ECR <br> - Kiểm tra rolling deployment để xác minh blue-green pattern | 25/09/2025 | 28/09/2025 | — |

### Những thành tích của Tuần 5

- Xây dựng và push thành công Docker images lên ECR repositories.
- Tạo reusable ECS Fargate task definitions với proper resource allocation.
- Triển khai containerized services và tích hợp với Application Load Balancer.
- Cấu hình service auto-scaling policies cho CPU và memory metrics.
- Kiểm tra rolling deployments mà không có service downtime.
- Học best practices về image tagging cho version control và deployment.

### Những thách thức và bài học

- ECR repository permissions phải được cấu hình chính xác cho push/pull operations.
- Task definition compatibility giữa local Docker và Fargate runtime yêu cầu chú ý tới base images.
- Service updates có thể tốn thời gian; cân nhắc sử dụng service deployment configuration parameters.
