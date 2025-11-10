---
title: "Bản đề xuất"
date: "2025-01-01"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Đề xuất Hệ thống Hỗ trợ Cai thuốc lá

## 1. Tóm tắt điều hành

Đề xuất này trình bày thiết kế và triển khai **Nền tảng Hỗ trợ Cai thuốc lá** dựa trên đám mây, nhằm giúp người dùng bỏ thuốc lá thông qua theo dõi dữ liệu, phân tích hành vi, huấn luyện AI và sự tương tác cộng đồng.  
Hệ thống tích hợp cơ sở hạ tầng backend hiện đại, có khả năng mở rộng được triển khai trên **AWS Cloud**, đảm bảo tính sẵn sàng cao, bảo mật và trải nghiệm người dùng liền mạch.

Mục tiêu là cung cấp một hành trình thông minh, được cá nhân hóa để người dùng theo dõi, lập kế hoạch và đạt được mục tiêu cai thuốc lá—đồng thời cung cấp cho quản trị viên và huấn luyện viên sức khỏe các công cụ để hỗ trợ và hướng dẫn họ.

---

## 2. Mục tiêu Hệ thống

- Giúp người dùng xây dựng và tuân theo kế hoạch cai thuốc lá được cá nhân hóa.
- Theo dõi hành vi hút thuốc và tiến trình sức khỏe trong thời gian thực.
- Cung cấp huấn luyện viên AI, lời nhắc và phản hồi động viên.
- Cho phép tương tác xã hội và khích lệ giữa các thành viên.
- Cung cấp cơ sở hạ tầng cloud-native an toàn để đảm bảo khả năng mở rộng và độ tin cậy.

---

## 3. Tính năng Chính

### Tính năng Hướng đến Người dùng

- **Trang chủ & Trung tâm Tri thức:** Trang chủ công khai giới thiệu nền tảng, chia sẻ câu chuyện thành công, FAQ và blog để chia sẻ kinh nghiệm và nội dung giáo dục.
- **Đăng ký, Gói Thành viên & Thanh toán:** Người dùng đăng ký, chọn gói thành viên (miễn phí/cao cấp) và hoàn tất thanh toán qua nhà cung cấp thanh toán bên thứ ba bảo mật; trạng thái đăng ký điều khiển quyền truy cập tính năng.
- **Theo dõi Tình trạng Hút thuốc:** Ghi lại lượng tiêu thụ thuốc lá hàng ngày, tần suất, nhãn hiệu/chi phí và yếu tố kích hoạt.
- **Tạo Kế hoạch Cai thuốc & Phân nhánh Hành trình:** Tạo kế hoạch cai thuốc (lý do, giai đoạn, ngày bắt đầu, ngày mục tiêu cai). Hệ thống đề xuất kế hoạch mà người dùng có thể tùy chỉnh. Nếu tái phạm hoặc bỏ cuộc xảy ra, hành trình phân nhánh với các bước phục hồi phù hợp và các mốc quan trọng được điều chỉnh.
- **Theo dõi Tiến trình & Thông tin Chi tiết:** Hiển thị số ngày không hút thuốc, tiền tiết kiệm được, cải thiện sức khỏe, chuỗi thành tích và biểu đồ xu hướng.
- **Thông báo Động viên Định kỳ:** Lời nhắc hàng ngày/hàng tuần và tin nhắn động viên qua ứng dụng, email hoặc SMS.
- **Thành tích & Huy hiệu (Có thể Chia sẻ):** Đạt được các mốc quan trọng (ví dụ: "1 Ngày Không Hút thuốc", "Tiết kiệm ₫100K") và chia sẻ huy hiệu lên bảng tin cộng đồng.
- **Tương tác Cộng đồng:** Khuyến khích và trao đổi lời khuyên với các thành viên khác (bình luận, phản ứng), với kiểm duyệt cơ bản.
- **Trợ lý Huấn luyện AI & Lộ trình:** Hướng dẫn cá nhân hóa và các bài viết lộ trình/kiến thức có thể tham khảo được hỗ trợ bởi AI; bao gồm tuyên bố từ chối trách nhiệm về an toàn và chuyển giao cho huấn luyện viên con người khi cần thiết.
- **Tương tác với Huấn luyện viên:** Người dùng có thể trò chuyện hoặc lên lịch phiên video với huấn luyện viên nền tảng để được tư vấn.
- **Quản lý Hồ sơ Người dùng:** Chỉnh sửa hồ sơ, tùy chọn, mục tiêu, cài đặt thông báo và các tùy chọn quyền riêng tư/đồng ý.
- **Đánh giá & Phản hồi:** Đánh giá các phiên huấn luyện và nội dung; gửi phản hồi để cải thiện liên tục.

### Tính năng Quản trị & Vận hành

- **Bảng điều khiển & Báo cáo:** Giám sát chỉ số người dùng, mức độ tương tác và phân tích tác động sức khỏe.
- **Cổng Huấn luyện viên & Quản lý Ca:** Phân công khối lượng công việc, phân loại người dùng có nguy cơ và tiến hành tư vấn qua trò chuyện/video.
- **Khai báo Gói Thành viên & Giá cả:** Xác định và công bố các gói phí, lợi ích và chương trình khuyến mãi; quản lý các điều khoản và điều kiện.
- **Quản lý Thanh toán & Đăng ký:** Quản lý đăng ký, hóa đơn, hoàn tiền và thử lại thanh toán thất bại.
- **Quản lý Nội dung:** Quản lý bài viết trang chủ/blog, tin nhắn động viên, huy hiệu và nội dung giáo dục.
- **Quản lý Phản hồi & Đánh giá:** Theo dõi và phản hồi chỉ số hài lòng của người dùng và chất lượng nội dung.

---

## 4. Kiến trúc Hệ thống (AWS Cloud)

Hệ thống tận dụng các dịch vụ được quản lý bởi AWS để đảm bảo khả năng mở rộng và bảo mật, như được minh họa trong sơ đồ kiến trúc.

![Sơ đồ Kiến trúc Nền tảng](/images/2-Proposal/arch.drawio.png)

### Tầng Frontend

- **Amazon S3** lưu trữ trang web tĩnh và frontend ứng dụng web (React).
- **Amazon CloudFront** phân phối nội dung web toàn cầu và xử lý mã hóa SSL/TLS.

### Xác thực & Phân quyền

- **Amazon Cognito** quản lý đăng ký, đăng nhập và liên kết danh tính của người dùng, đảm bảo quyền truy cập an toàn cho cả người dùng cuối và huấn luyện viên.

### Tầng Ứng dụng

- **AWS Lambda** cung cấp các dịch vụ serverless như webhook thanh toán, công việc theo lịch và các hoạt động API nhẹ.
- **Trò chuyện/Video với Huấn luyện viên:** cho phép các phiên trò chuyện/video thời gian thực với huấn luyện viên.
- **Thanh toán:** Tích hợp với cổng thanh toán bên thứ ba (ví dụ: Stripe/VNPay/MoMo) qua webhook đã ký và luồng token hóa; khóa được lưu trữ trong **AWS Secrets Manager**.
- **Network Load Balancer (NLB)** phân phối yêu cầu trên các instance EC2 backend.
- **EC2 Instances (Private Subnet)** lưu trữ các microservices cốt lõi:
    - **User Service** (hồ sơ, tích hợp xác thực)
    - **Cessation Service** (kế hoạch, theo dõi, thành tích, phân nhánh tái phạm)
    - **Social Service** (bảng tin cộng đồng, bình luận, phản ứng)
    - **AI Orchestration Service** (lời nhắc huấn luyện AI và rào cản bảo vệ)

### Tầng Dữ liệu

- **PostgreSQL Databases** (Amazon EC2) cho dữ liệu người dùng, kế hoạch và giao dịch.
- **MongoDB** (Amazon EC2) cho các tính năng xã hội và nội dung phi cấu trúc.
- **S3 Buckets** lưu trữ tài sản (hình ảnh, xuất dữ liệu) và sao lưu cơ sở dữ liệu được mã hóa định kỳ.

### DevOps Pipeline

- **Code Pipeline** tự động hóa xây dựng, đóng gói dịch vụ vào container và đẩy image lên **Amazon ECR**, sau đó triển khai lên EC2/Auto Scaling groups.
- **VPC Endpoint** đảm bảo giao tiếp an toàn với các dịch vụ AWS mà không để lộ lưu lượng ra internet công cộng.
- **EC2 Instance Connect Endpoint** cho phép quyền truy cập quản trị được kiểm soát.

---

## 5. Bảo mật và Tuân thủ

- **Mã hóa Dữ liệu:** Tất cả dữ liệu nhạy cảm được mã hóa trong quá trình truyền (TLS) và khi lưu trữ (AES-256).
- **Chính sách IAM:** Kiểm soát quyền truy cập chi tiết cho các vai trò hệ thống khác nhau.
- **Private Subnets:** Backend và cơ sở dữ liệu được cách ly khỏi phơi bày công khai.
- **VPC Link & Endpoints:** Giao tiếp nội bộ an toàn giữa các dịch vụ.
- **Thanh toán:** Không có dữ liệu thẻ nào được lưu trữ trên nền tảng. Xử lý thẻ được ủy quyền cho nhà cung cấp thanh toán; bí mật được quản lý trong **AWS Secrets Manager**. Phạm vi PCI-DSS vẫn thuộc về nhà cung cấp.
- **Quyền riêng tư & Đồng ý:** Đồng ý của người dùng, kiểm soát quyền riêng tư hồ sơ và chính sách lưu giữ/vòng đời dữ liệu được thực thi.
- **Chiến lược Sao lưu:** Sao lưu tự động hàng ngày lên S3 với phiên bản và chính sách vòng đời.

---

## 6. Khả năng Mở rộng và Hiệu suất

- **Auto Scaling:** EC2 và Lambda functions mở rộng dựa trên nhu cầu người dùng.
- **CDN Caching:** CloudFront lưu cache nội dung tĩnh để phân phối nhanh hơn trên toàn cầu.
- **Load Balancing:** NLB đảm bảo phân phối lưu lượng và khả năng chịu lỗi.
- **Microservices Tách rời:** Thiết kế module cho phép mở rộng độc lập các dịch vụ người dùng, cai thuốc, xã hội và thông báo.
- **Workloads Hướng sự kiện:** EventBridge tách rời lên lịch thông báo, phát hành huy hiệu và quy trình phân nhánh tái phạm.

---

## 7. Ước tính Chi phí

**Những gì MIỄN PHÍ ($0.00)**:

- 2 Lambda Functions: Free Tier bao gồm 1 triệu yêu cầu và 400,000 GB-giây thời gian tính toán mỗi tháng. Điều này là quá đủ cho bản demo.

- 2 S3 Buckets: Free Tier bao gồm 5 GB lưu trữ S3 Standard.

- 1 ECR Repository: Free Tier bao gồm 500 MB mỗi tháng lưu trữ cho kho riêng tư.

- 1 CloudFront Distribution: Free Tier bao gồm 1 TB truyền dữ liệu ra và 10 triệu yêu cầu HTTP/S mỗi tháng.

- 1 EC2 Instance: Free Tier bao gồm 750 giờ/tháng của instance t4g.micro.

- 1 CodePipeline: Free Tier bao gồm 1 pipeline hoạt động miễn phí mỗi tháng.

- Cognito User Pool: Free Tier bao gồm 50,000 người dùng hoạt động hàng tháng.

**Những gì không Miễn phí (Chi phí Ước tính Hàng tháng)**:

- 3 EC2 Instances (t4g.small) cho microservices và cơ sở dữ liệu: ~$45

- 1 Network Load Balancer: ~$18

- 1 VPC Endpoint: ~$5

**Tổng Chi phí Ước tính Hàng tháng**: ~$68

---

## 8. Kết quả Kỳ vọng

- Tăng tỷ lệ cai thuốc lá thành công.
- Cải thiện động lực và sự tương tác của người dùng.
- Nền tảng có khả năng mở rộng có thể hỗ trợ cơ sở người dùng lớn.
- Cơ sở hạ tầng cloud an toàn, tuân thủ và dễ bảo trì.

---

