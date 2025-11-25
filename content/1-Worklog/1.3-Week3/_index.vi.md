---
title: "Worklog Tuần 3"
date: "2025-09-21"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu Tuần 3

- Thiết lập ứng dụng Angular với HTTPS cục bộ.
- Tích hợp AWS Cognito, S3, API Gateway, Lambda, CloudWatch.
- Thiết lập baseline logging, metrics, và auth flows.

### Các công việc
| Ngày | Công việc | Ngày dự kiến | Ngày hoàn thành | Tài liệu tham khảo |
|------|-------------------------------------------------------------------------------------------------------|--------------|-----------|------------|
| Thứ 2 | Khởi tạo dự án Angular <br> Thêm chứng chỉ tự ký <br> Ghi chép thiết lập HTTPS | 2025-09-09 | 2025-09-09 | |
| Thứ 3 | Cấu hình AWS Cognito user pool <br> Triển khai login/logout & route guards <br> Xử lý làm mới token | 2025-09-10 | 2025-09-10 | |
| Thứ 4 | Xây dựng dịch vụ S3 với presigned URL upload/download <br> Thêm retry + thông báo lỗi | 2025-09-11 | 2025-09-11 | |
| Thứ 5 | Tạo Lambda + API Gateway endpoints | 2025-09-12 | 2025-09-12 | |
| Thứ 6 | Làm việc trên đề xuất | 2025-09-13 | 2025-09-13 | |
| Thứ 7 | Làm việc trên đề xuất | 2025-09-14 | 2025-09-14 | |

### Những thành tích

- Ứng dụng Angular phát triển cục bộ qua HTTPS.
- Xác thực Cognito (đăng nhập, làm mới, các route được bảo vệ).
- Quy trình làm việc upload/download tệp S3 an toàn.
- Backend Lambda + API Gateway được tiêu thụ bởi frontend.
- Thêm logging + metrics ban đầu (invocation counts, basic latency).
- Giảm các chính sách IAM quá rộng.
- Tăng test coverage frontend từ ~15% lên ~40%.
- Ghi lại các hiểu biết hành động từ sự kiện AWS.

### Những trở ngại / Giải pháp khắc phục

- Cấu hình CORS sai trên API Gateway → điều chỉnh các origins/headers được phép.
- Các lời nhắc tin tưởng chứng chỉ tự ký → thêm phần thiết lập README.
- Độ trễ upload tệp lớn → thêm chunked upload vào backlog Tuần 4.
