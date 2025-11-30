---
title: "Worklog Tuần 10"
date: "2025-11-09"
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu Tuần 10

- Tối ưu hóa S3 storage và triển khai lifecycle policies.
- Cấu hình S3 cho static website hosting với CloudFront.
- Triển khai data encryption và access controls cho S3 buckets.

### Các công việc thực hiện trong tuần này
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|-----|------|------------|-----------------|--------------------|
| Thứ 2 | - Xem xét S3 bucket structure và storage classes <br> - Lập kế hoạch lifecycle policies cho cost optimization | 30/10/2025 | 02/11/2025 | <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/">Tài liệu S3</a> |
| Thứ 3 | - Weekly Scrum/Meeting: Báo cáo tiến độ và đồng bộ hóa với đội | 30/10/2025 | 02/11/2025 | — |
| Thứ 4 | - Bật server-side encryption (SSE-S3) cho tất cả objects <br> - Cấu hình bucket policies cho access control | 30/10/2025 | 02/11/2025 | <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/security.html">S3 Security</a> |
| Thứ 5 | - Bật S3 access logging và cấu hình CloudTrail cho audit <br> - Thiết lập bucket inventory cho analysis | 30/10/2025 | 02/11/2025 | <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html">Logging Guide</a> |
| Thứ 6 | - Triển khai presigned URLs cho temporary object access <br> - Kiểm tra access control và permissions | 30/10/2025 | 02/11/2025 | <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html">Presigned URLs</a> |
| Thứ 7 | - Ước tính cost savings từ lifecycle policies <br> - Tài liệu hóa S3 bucket configuration và access patterns | 30/10/2025 | 02/11/2025 | — |

### Những thành tích của Tuần 10

- Triển khai S3 lifecycle policies để chuyển old objects tới Glacier storage.
- Cấu hình bucket encryption và access logging cho compliance.
- Thiết lập presigned URLs cho secure, time-limited object access.
- Bật versioning và bucket inventory cho data management.
- Tính toán potential 40% cost reduction thông qua storage class optimization.
- Học S3 bucket policies, IAM roles, và access control strategies.

### Những thách thức và bài học

- Lifecycle transitions có minimum object age requirements; lập kế hoạch tương ứng.
- Presigned URL generation yêu cầu proper IAM permissions cho generating role.
- Bật versioning tăng storage costs do retained object versions.
