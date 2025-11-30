---
title: "Worklog Tuần 3"
date: "2025-09-21"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu Tuần 3

- Thiết lập ứng dụng Angular với HTTPS cục bộ.
- Tích hợp AWS Cognito, S3, API Gateway, Lambda, CloudWatch.
- Thiết lập baseline logging, metrics, và auth flows.

### Các công việc thực hiện trong tuần này
| Ngày | Công việc | Ngày dự kiến | Ngày hoàn thành | Tài liệu tham khảo |
|------|-------------------------------------------------------------------------------------------------------|--------------|-----------|------------|
| Thứ 2 | - Khởi tạo project Angular <br> - Thêm chứng chỉ tự ký <br> - Tài liệu hóa thiết lập HTTPS | 2025-09-09 | 2025-09-09 | — |
| Thứ 3 | - Cấu hình AWS Cognito user pool <br> - Triển khai login/logout & route guards <br> - Xử lý token refresh | 2025-09-10 | 2025-09-10 | — |
| Thứ 4 | - Weekly Scrum/Meeting: Báo cáo tiến độ và đồng bộ hóa với đội | 2025-09-11 | 2025-09-11 | — |
| Thứ 5 | - Tạo Lambda + API Gateway endpoints | 2025-09-12 | 2025-09-12 | — |
| Thứ 6 | - Làm việc trên đề xuất | 2025-09-13 | 2025-09-13 | — |
| Thứ 7 | - Làm việc trên đề xuất | 2025-09-14 | 2025-09-14 | — |

### Những thành tích của Tuần 3

- Angular dev cục bộ qua HTTPS.
- Cognito auth (login, refresh, protected routes).
- S3 secure file upload/download workflow.
- Lambda + API Gateway backend được sử dụng bởi frontend.
- Thêm logging + initial metrics (invocation counts, basic latency).
- Giảm các IAM policies quá rộng.
- Tăng test coverage của frontend từ ~15% lên ~40%.
- Ghi lại những insights hữu ích từ sự kiện AWS.

### Những chướng ngại và giải pháp

- CORS misconfiguration trên API Gateway → điều chỉnh allowed origins/headers.
- Self-signed cert trust prompts → thêm phần thiết lập README.
- Large file upload latency → thêm chunked upload vào backlog Tuần 4.
