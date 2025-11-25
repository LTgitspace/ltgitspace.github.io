---
title: "Worklog Tuần 1"
date: "2025-09-14"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

### Mục tiêu Tuần 1

- Kết nối và làm quen với các thành viên First Cloud Journey (FCJ).
- Hiểu các danh mục dịch vụ AWS cơ bản và học cách sử dụng cả Console và AWS CLI.

### Các công việc thực hiện trong tuần này
| Ngày | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|-----|------|------------|-----------------|--------------------|
| Thứ 2 | - Làm quen với các thành viên FCJ <br> - Đọc và nắm vững các quy tắc và quy định thực tập | 11/09/2025 | 14/09/2025 | — |
| Thứ 3 | - Tổng quan về AWS và các nhóm dịch vụ cốt lõi <br>&emsp; • Compute <br>&emsp; • Storage <br>&emsp; • Networking <br>&emsp; • Database | 11/09/2025 | 14/09/2025 | <a href="https://aws.amazon.com/what-is-aws/">AWS là gì?</a> · <a href="https://aws.amazon.com/products/">Các sản phẩm AWS</a> |
| Thứ 4 | - Tạo tài khoản AWS Free Tier <br> - Học AWS Console & AWS CLI <br> - Thực hành: <br>&emsp; • Tạo tài khoản AWS <br>&emsp; • Cài đặt & cấu hình AWS CLI <br>&emsp; • Chạy các lệnh CLI cơ bản | 11/09/2025 | 14/09/2025 | <a href="https://aws.amazon.com/free/">AWS Free Tier</a> · <a href="https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html">Cài đặt AWS CLI</a> · <a href="https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html">Cấu hình CLI</a> |
| Thứ 5 | - Học các kiến thức cơ bản về EC2: <br>&emsp; • Các loại Instance <br>&emsp; • AMIs <br>&emsp; • EBS <br>&emsp; • Key pairs & security groups <br> - Các phương pháp SSH để kết nối EC2 <br> - Bắt đầu code một trang báo cáo FCJ cá nhân trong khi học Angular | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/ec2/">Tài liệu EC2</a> · <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html">Kết nối EC2</a> · <a href="https://angular.dev/overview">Tổng quan Angular</a> |
| Thứ 6 | - Thực hành: <br>&emsp; • Khởi chạy một instance EC2 <br>&emsp; • Kết nối qua SSH <br>&emsp; • Gắn một volume EBS | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html">Khởi chạy EC2</a> · <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-to-linux-instance.html">SSH vào EC2</a> · <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html">Gắn EBS</a> |
| Thứ 7 | - Tạo một bucket S3 và cấu hình static website hosting <br> - Triển khai trang FCJ tĩnh cá nhân | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html">S3 static website</a> |

### Những thành tích của Tuần 1

- Có hiểu biết vững chắc về các kiến thức cơ bản của AWS và các nhóm dịch vụ chính: Compute, Storage, Networking, và Database.
- Đã tạo và cấu hình thành công một tài khoản AWS Free Tier.
- Làm quen với AWS Management Console và cách tìm, truy cập, và sử dụng các dịch vụ thông qua giao diện web.
- Cài đặt và cấu hình AWS CLI, bao gồm Access Key, Secret Key, và các cài đặt Region mặc định.
- Khởi chạy một instance EC2 thử nghiệm, kết nối qua SSH, và gắn một volume EBS.
- Triển khai một trang web tĩnh đơn giản đến Amazon S3 để báo cáo FCJ.

### Những thách thức và bài học

- Các quyền IAM và quản lý khóa yêu cầu xử lý cẩn thận; nguyên tắc least-privilege là rất quan trọng.
- Security groups và cài đặt networking (inbound/outbound rules) ảnh hưởng trực tiếp đến khả năng kết nối; xác thực chúng trước tiên khi SSH gặp sự cố.
- Lựa chọn Region ảnh hưởng đến tính khả dụng của dịch vụ và giá cả—chuẩn hóa một Region chính từ sớm.
