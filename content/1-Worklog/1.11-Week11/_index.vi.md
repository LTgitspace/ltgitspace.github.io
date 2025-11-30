---
title: "Worklog Tuần 11"
date: "2025-11-16"
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu Tuần 11

- Triển khai comprehensive monitoring sử dụng CloudWatch.
- Tạo dashboards và alarms cho application health.
- Thiết lập logging aggregation và analysis.

### Các công việc thực hiện trong tuần này
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|-----|------|------------|-----------------|--------------------|
| Thứ 2 | - Tạo CloudWatch dashboards cho key metrics <br> - Thiết lập custom metrics từ application logs | 06/11/2025 | 09/11/2025 | <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/">Tài liệu CloudWatch</a> |
| Thứ 3 | - Weekly Scrum/Meeting: Báo cáo tiến độ và đồng bộ hóa với đội | 06/11/2025 | 09/11/2025 | — |
| Thứ 4 | - Bật detailed CloudWatch logs cho ECS tasks <br> - Tạo log groups và cấu hình retention policies | 06/11/2025 | 09/11/2025 | <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/">Logs Guide</a> |
| Thứ 5 | - Thiết lập CloudWatch Insights cho log analysis <br> - Tạo queries cho common troubleshooting scenarios | 06/11/2025 | 09/11/2025 | <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html">CloudWatch Insights</a> |
| Thứ 6 | - Cấu hình ứng dụng để gửi custom metrics tới CloudWatch <br> - Kiểm tra alarm triggering và notification flow | 06/11/2025 | 09/11/2025 | <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html">Metrics Guide</a> |
| Thứ 7 | - Xem xét và tối ưu hóa dashboard layout <br> - Tài liệu hóa monitoring architecture và alert thresholds | 06/11/2025 | 09/11/2025 | — |

### Những thành tích của Tuần 11

- Tạo comprehensive CloudWatch dashboards hiển thị system health.
- Cấu hình proactive alarms cho key performance indicators (CPU, memory, error rates).
- Triển khai centralized logging cho tất cả ECS tasks và services.
- Thiết lập CloudWatch Insights queries cho rapid troubleshooting.
- Xác minh SNS notifications delivery cho critical alerts.
- Đạt real-time visibility vào application và infrastructure performance.

### Những thách thức và bài học

- Custom metrics phát sinh thêm CloudWatch costs; hãy selective về điều cần monitoring.
- Log retention policies nên cân bằng storage costs với compliance requirements.
- Alarm thresholds yêu cầu tuning để tránh false positives và alert fatigue.
