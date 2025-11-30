---
title: "Worklog Tuần 6"
date: "2025-10-12"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu Tuần 6

- Thiết lập continuous integration và deployment (CI/CD) pipeline sử dụng AWS CodePipeline.
- Tích hợp GitHub repository với CodePipeline cho automated deployments.
- Triển khai automated testing trong pipeline.

### Các công việc thực hiện trong tuần này
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|-----|------|------------|-----------------|--------------------|
| Thứ 2 | - Tạo CodePipeline project <br> - Cấu hình GitHub source connection và credentials | 02/10/2025 | 05/10/2025 | <a href="https://docs.aws.amazon.com/codepipeline/latest/userguide/">Tài liệu CodePipeline</a> |
| Thứ 3 | - Weekly Scrum/Meeting: Báo cáo tiến độ và đồng bộ hóa với đội | 02/10/2025 | 05/10/2025 | — |
| Thứ 4 | - Cấu hình ECR làm deployment target <br> - Tạo deployment stage trong CodePipeline | 02/10/2025 | 05/10/2025 | <a href="https://docs.aws.amazon.com/codepipeline/latest/userguide/integrations.html">CodePipeline Integrations</a> |
| Thứ 5 | - Thêm manual approval step trước production deployment <br> - Kiểm tra pipeline execution với code commits | 02/10/2025 | 05/10/2025 | <a href="https://docs.aws.amazon.com/codepipeline/latest/userguide/concepts.html">CodePipeline Concepts</a> |
| Thứ 6 | - Thêm CloudWatch notifications cho pipeline events <br> - Tài liệu hóa pipeline stages và workflow | 02/10/2025 | 05/10/2025 | <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/">CloudWatch Events Guide</a> |
| Thứ 7 | - Kiểm tra end-to-end deployment flow <br> - Xác minh ECR image updates và ECS service deployment | 02/10/2025 | 05/10/2025 | — |

### Những thành tích của Tuần 6

- Tạo thành công CI/CD pipeline hoạt động từ GitHub tới ECS.
- Automated Docker image building và ECR push qua CodeBuild.
- Triển khai manual approval gates cho production deployments.
- Thiết lập CloudWatch notifications cho pipeline state changes.
- Giảm manual deployment steps từ 8+ steps xuống 1 git push.
- Học best practices cho pipeline về failure handling và rollback scenarios.

### Những thách thức và bài học

- GitHub token authentication yêu cầu proper IAM policies cho CodePipeline.
- CodeBuild environment variables phải bao gồm AWS credentials cho ECR push operations.
- Pipeline failures có thể khó hiểu; CloudWatch logs là cần thiết cho debugging.
