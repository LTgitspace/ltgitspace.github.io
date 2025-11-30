---
title: "Worklog Tuần 1"
date: "2025-09-14"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu Tuần 1

- Kết nối và làm quen với các thành viên First Cloud Journey (FCJ).
- Hiểu các danh mục dịch vụ AWS chính và học cách sử dụng cả AWS Console lẫn AWS CLI.

### Các công việc thực hiện trong tuần này
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|-----|------|------------|-----------------|--------------------|
| Thứ 2 | - Tham gia onboarding với các thành viên FCJ <br> - Đọc và nắm rõ các quy tắc và quy định thực tập | 11/09/2025 | 14/09/2025 | — |
| Thứ 3 | - Weekly Scrum/Meeting: Báo cáo tiến độ và đồng bộ hóa với đội | 11/09/2025 | 14/09/2025 | — |
| Thứ 4 | - Tạo tài khoản AWS Free Tier <br> - Học AWS Console & AWS CLI <br> - Thực hành: <br>&emsp; • Tạo tài khoản AWS <br>&emsp; • Cài đặt & cấu hình AWS CLI <br>&emsp; • Chạy các lệnh CLI cơ bản | 11/09/2025 | 14/09/2025 | <a href="https://aws.amazon.com/free/">AWS Free Tier</a> · <a href="https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html">Cài đặt AWS CLI</a> · <a href="https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html">Cấu hình CLI</a> |
| Thứ 5 | - Học những kiến thức cơ bản về EC2: <br>&emsp; • Loại instance <br>&emsp; • AMIs <br>&emsp; • EBS <br>&emsp; • Key pairs & security groups <br> - Các phương pháp SSH cho EC2 <br> - Bắt đầu lập trình trang báo cáo FCJ cá nhân trong khi học Angular | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/ec2/">Tài liệu EC2</a> · <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html">Kết nối EC2</a> · <a href="https://angular.dev/overview">Tổng quan Angular</a> |
| Thứ 6 | - Thực hành: <br>&emsp; • Khởi chạy instance EC2 <br>&emsp; • Kết nối qua SSH <br>&emsp; • Gắn volume EBS | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html">Khởi chạy EC2</a> · <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-to-linux-instance.html">SSH tới EC2</a> · <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html">Gắn EBS</a> |
| Thứ 7 | - Tạo bucket S3 và cấu hình static website hosting <br> - Triển khai trang web static FCJ cá nhân | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html">S3 static website</a> |

### Những thành tích của Tuần 1

- Đạt được sự hiểu biết vững chắc về những kiến thức cơ bản của AWS và các nhóm dịch vụ chính: Compute, Storage, Networking, và Database.
- Tạo và cấu hình thành công tài khoản AWS Free Tier.
- Làm quen với AWS Management Console và cách tìm, truy cập, và sử dụng các dịch vụ thông qua giao diện web.
- Cài đặt và cấu hình AWS CLI, bao gồm Access Key, Secret Key, và cài đặt Region mặc định.
- Khởi chạy instance EC2 thử nghiệm, kết nối qua SSH, và gắn volume EBS.
- Triển khai website tĩnh đơn giản lên Amazon S3 để báo cáo FCJ.

### Những thách thức và bài học

- Quyền IAM và quản lý khóa yêu cầu xử lý cẩn thận; nguyên tắc least-privilege là cần thiết.
- Security groups và cài đặt networking (inbound/outbound rules) trực tiếp ảnh hưởng tới kết nối; xác thực chúng trước tiên khi SSH gặp sự cố.
- Lựa chọn region ảnh hưởng tới tính khả dụng của dịch vụ và giá cả—tiêu chuẩn hóa trên một region chính từ đầu.
