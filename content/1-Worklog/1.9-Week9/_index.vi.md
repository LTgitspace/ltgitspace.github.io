---
title: "Worklog Tuần 9"
date: "2025-11-02"
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu Tuần 9

- Thiết lập DynamoDB cho NoSQL data storage và caching.
- Triển khai DynamoDB tables với proper indexing strategies.
- Tích hợp DynamoDB với ứng dụng cho session management.

### Các công việc thực hiện trong tuần này
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|-----|------|------------|-----------------|--------------------|
| Thứ 2 | - Tạo DynamoDB tables cho application data <br> - Xác định primary keys và attributes | 23/10/2025 | 26/10/2025 | <a href="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/">Tài liệu DynamoDB</a> |
| Thứ 3 | - Weekly Scrum/Meeting: Báo cáo tiến độ và đồng bộ hóa với đội | 23/10/2025 | 26/10/2025 | — |
| Thứ 4 | - Cấu hình on-demand billing mode vs provisioned capacity <br> - Theo dõi DynamoDB throttling và capacity | 23/10/2025 | 26/10/2025 | <a href="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/BillingMode.html">Billing Mode Guide</a> |
| Thứ 5 | - Cập nhật application code để tích hợp DynamoDB operations <br> - Triển khai session storage sử dụng DynamoDB | 23/10/2025 | 26/10/2025 | <a href="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html">Query Operations</a> |
| Thứ 6 | - Kiểm tra CRUD operations (Create, Read, Update, Delete) <br> - Load test để xác minh performance dưới concurrent requests | 23/10/2025 | 26/10/2025 | <a href="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html">Best Practices</a> |
| Thứ 7 | - Bật point-in-time recovery <br> - Theo dõi và tối ưu hóa query patterns | 23/10/2025 | 26/10/2025 | — |

### Những thành tích của Tuần 9

- Tạo thành công DynamoDB tables với optimized key design.
- Triển khai Global Secondary Indexes cho flexible querying.
- Cấu hình TTL cho automatic session data cleanup.
- Tích hợp DynamoDB session storage vào ứng dụng.
- Kiểm tra CRUD operations và xác minh performance metrics.
- Học DynamoDB partition key design và hot partition mitigation.

### Những thách thức và bài học

- Hot partitions có thể gây throttling; phân phối write load trên partition keys.
- Query costs thay đổi dựa trên index utilization; tối ưu hóa attribute selection trong queries.
- On-demand vs provisioned capacity trade-off phụ thuộc vào workload predictability.
