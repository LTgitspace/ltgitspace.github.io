---
title: "Blog 3"
date: "2025-01-01"
weight: 1
chapter: false
pre: " <b> 3.6. </b> "
---

Xử lý thế chấp tự động bằng Amazon Bedrock Data Automation và Amazon Bedrock Agents
Đăng bởi Wrick Talukdar, Jessie-Lee Fry, Farshad Bidanjiri, Jady Liu, Keith Mascarenhas, và Raj Jayaraman | ngày 01 tháng 5 năm 2025
Xử lý thế chấp là một quy trình làm việc phức tạp, đòi hỏi nhiều tài liệu và yêu cầu độ chính xác, hiệu quả và tuân thủ. Các hoạt động thế chấp truyền thống dựa vào việc xem xét thủ công, tự động hóa dựa trên quy tắc và các hệ thống riêng rẽ, thường dẫn đến sự chậm trễ, sai sót và trải nghiệm khách hàng kém. Các cuộc khảo sát ngành gần đây cho thấy chỉ khoảng một nửa số người vay bày tỏ sự hài lòng với quy trình thế chấp, trong đó các ngân hàng truyền thống tụt hậu so với các bên cho vay phi ngân hàng về mức độ hài lòng của người vay. Khoảng cách về mức độ hài lòng này phần lớn là do bản chất thủ công, dễ sai sót của quy trình xử lý thế chấp truyền thống, nơi sự chậm trễ, không nhất quán và quy trình làm việc rời rạc tạo ra sự thất vọng cho người vay và ảnh hưởng đến trải nghiệm tổng thể.
Trong bài đăng này, chúng tôi giới thiệu phê duyệt thế chấp tự động dựa trên tác nhân (agentic automatic mortgage approval), một giải pháp mẫu thế hệ tiếp theo sử dụng các tác nhân AI tự trị được cung cấp bởi Amazon Bedrock Agents và Amazon Bedrock Data Automation. Các tác nhân này điều phối toàn bộ quy trình phê duyệt thế chấp—xác minh tài liệu một cách thông minh, đánh giá rủi ro và đưa ra quyết định dựa trên dữ liệu với sự can thiệp tối thiểu của con người. Bằng cách tự động hóa các quy trình làm việc phức tạp, các doanh nghiệp có thể đẩy nhanh quá trình phê duyệt, giảm thiểu sai sót và cung cấp tính nhất quán đồng thời nâng cao khả năng mở rộng và tuân thủ.
Đoạn video sau đây cho thấy tự động hóa dựa trên tác nhân này hoạt động—cho phép xử lý thế chấp thông minh hơn, nhanh hơn và đáng tin cậy hơn ở quy mô lớn.
https://d2908q01vomqb2.cloudfront.net/artifacts/DBSBlogs/ML-18689/agentic-demo-video.mp4

Tại sao lại là IDP dựa trên agent?
Xử lý tài liệu thông minh dựa trên tác nhân (Agentic intelligent document processing - IDP) cách mạng hóa quy trình làm việc với tài liệu bằng cách thúc đẩy hiệu quả và tính tự chủ. Nó tự động hóa các tác vụ với độ chính xác, cho phép các hệ thống trích xuất, phân loại và xử lý thông tin đồng thời xác định và sửa lỗi trong thời gian thực. IDP dựa trên tác nhân không chỉ dừng lại ở việc trích xuất đơn giản bằng cách nắm bắt ngữ cảnh và ý định, bổ sung thêm thông tin chi tiết sâu sắc hơn vào các tài liệu để thúc đẩy việc ra quyết định thông minh hơn. Được cung cấp bởi Amazon Bedrock Data Automation, nó thích ứng với các định dạng tài liệu và nguồn dữ liệu thay đổi, giúp giảm hơn nữa công việc thủ công.
Được xây dựng để có tốc độ và quy mô, IDP dựa trên tác nhân xử lý khối lượng lớn tài liệu một cách nhanh chóng, giảm sự chậm trễ và tối ưu hóa các hoạt động kinh doanh quan trọng. Tích hợp liền mạch với các tác nhân AI và hệ thống doanh nghiệp, nó tự động hóa các quy trình làm việc phức tạp, cắt giảm chi phí vận hành và giải phóng các nhóm để tập trung vào các sáng kiến chiến lược có giá trị cao.
IDP trong xử lý thế chấp

Xử lý thế chấp bao gồm nhiều bước, bao gồm khởi tạo khoản vay, xác minh tài liệu, thẩm định và kết thúc; mỗi bước đòi hỏi nỗ lực thủ công đáng kể. Các bước này thường không liên kết với nhau, dẫn đến thời gian xử lý chậm (hàng tuần thay vì hàng phút), chi phí hoạt động cao (xem xét tài liệu thủ công) và tăng nguy cơ sai sót của con người và gian lận. Các tổ chức phải đối mặt với nhiều thách thức kỹ thuật khi quản lý thủ công các quy trình làm việc nhiều tài liệu, như được mô tả trong sơ đồ sau.
Những thách thức này bao gồm:
Quá tải tài liệu – Đơn xin thế chấp yêu cầu xác minh tài liệu rộng rãi, bao gồm hồ sơ thuế, báo cáo thu nhập, thẩm định tài sản và các thỏa thuận pháp lý. Ví dụ, một đơn xin thế chấp duy nhất có thể yêu cầu xem xét và đối chiếu thủ công hàng trăm trang tờ khai thuế, phiếu lương, sao kê ngân hàng và tài liệu pháp lý, tiêu tốn thời gian và nguồn lực đáng kể.
Lỗi nhập dữ liệu – Xử lý thủ công gây ra sự không nhất quán, không chính xác và thiếu thông tin trong quá trình nhập dữ liệu. Việc chép sai thu nhập của người nộp đơn từ các biểu mẫu W-2 hoặc giải thích sai dữ liệu thẩm định tài sản có thể dẫn đến tính toán sai điều kiện vay, đòi hỏi phải sửa chữa và làm lại tốn kém.
Chậm trễ trong việc ra quyết định – Tình trạng tồn đọng do các quy trình xem xét thủ công kéo dài thời gian xử lý và ảnh hưởng tiêu cực đến sự hài lòng của người vay. Một bên cho vay xem xét thủ công việc xác minh thu nhập và tài liệu tín dụng có thể mất vài tuần để giải quyết tình trạng tồn đọng của họ, gây ra sự chậm trễ dẫn đến mất cơ hội hoặc người nộp đơn thất vọng chuyển sang đối thủ cạnh tranh.
Sự phức tạp trong việc tuân thủ quy định – Các quy định của ngành thế chấp đang phát triển làm tăng thêm sự phức tạp cho các thủ tục thẩm định và xác minh. Những thay đổi trong các quy định cho vay, chẳng hạn như các yêu cầu công bố thông tin bắt buộc mới hoặc các hướng dẫn xác minh thu nhập được cập nhật, có thể yêu cầu cập nhật thủ công rộng rãi cho các quy trình, dẫn đến thời gian xử lý tăng, chi phí hoạt động cao hơn và tỷ lệ lỗi tăng do nhập dữ liệu thủ công.
Những thách thức này nhấn mạnh sự cần thiết của tự động hóa để nâng cao hiệu quả, tốc độ và độ chính xác cho cả người cho vay và người vay thế chấp.
Giải pháp: Quy trình làm việc dựa trên tác nhân trong xử lý thế chấp
Giải pháp sau đây là một giải pháp khép kín và người nộp đơn chỉ tương tác với tác nhân giám sát người nộp đơn thế chấp để tải lên tài liệu và kiểm tra hoặc truy xuất trạng thái đơn. Sơ đồ sau đây minh họa quy trình làm việc.

Quy trình làm việc bao gồm các bước sau:
Người nộp đơn tải lên tài liệu để đăng ký thế chấp.
Tác nhân giám sát xác nhận đã nhận được tài liệu.
Người nộp đơn có thể xem và truy xuất trạng thái đơn.
Người thẩm định cập nhật trạng thái của đơn và gửi tài liệu phê duyệt cho người nộp đơn.
Cốt lõi của quy trình xử lý thế chấp dựa trên tác nhân là một tác nhân giám sát (supervisor agent) điều phối toàn bộ quy trình làm việc, quản lý các tác nhân phụ và đưa ra quyết định cuối cùng. Amazon Bedrock Agents là một khả năng trong Amazon Bedrock cho phép các nhà phát triển tạo ra các trợ lý được hỗ trợ bởi AI có khả năng hiểu các yêu cầu của người dùng và thực hiện các tác vụ phức tạp. Các tác nhân này có thể chia nhỏ các yêu cầu thành các bước logic, tương tác với các công cụ và nguồn dữ liệu bên ngoài, đồng thời sử dụng các mô hình AI để suy luận và hành động. Chúng duy trì bối cảnh cuộc trò chuyện trong khi kết nối an toàn với các API và dịch vụ AWS khác nhau, khiến chúng trở nên lý tưởng cho các tác vụ như tự động hóa dịch vụ khách hàng, phân tích dữ liệu và tự động hóa quy trình kinh doanh.
Tác nhân giám sát ủy quyền các nhiệm vụ một cách thông minh cho các tác nhân phụ chuyên biệt trong khi vẫn duy trì sự cân bằng phù hợp giữa xử lý tự động và giám sát của con người. Bằng cách tổng hợp thông tin chi tiết và dữ liệu từ các tác nhân phụ khác nhau, tác nhân giám sát áp dụng các quy tắc kinh doanh và tiêu chí rủi ro đã được thiết lập để tự động phê duyệt các khoản vay đủ điều kiện hoặc gắn cờ các trường hợp phức tạp để con người xem xét, cải thiện cả hiệu quả và độ chính xác trong quy trình thẩm định thế chấp.
Trong các phần sau, chúng ta sẽ tìm hiểu chi tiết hơn về các tác nhân phụ.
Tác nhân trích xuất dữ liệu (Data extraction agent)
Tác nhân trích xuất dữ liệu sử dụng Amazon Bedrock Data Automation để trích xuất các thông tin quan trọng từ các gói hồ sơ thế chấp, bao gồm phiếu lương, biểu mẫu W-2, sao kê ngân hàng và tài liệu nhận dạng. Amazon Bedrock Data Automation là một khả năng được hỗ trợ bởi AI tạo sinh của Amazon Bedrock giúp hợp lý hóa việc phát triển các ứng dụng AI tạo sinh và tự động hóa các quy trình làm việc liên quan đến tài liệu, hình ảnh, âm thanh và video.
Tác nhân trích xuất dữ liệu giúp đảm bảo rằng tác nhân xác thực, tuân thủ và ra quyết định nhận được dữ liệu chính xác và có cấu trúc, cho phép xác thực hiệu quả, tuân thủ quy định và ra quyết định sáng suốt. Sơ đồ sau đây minh họa quy trình làm việc.

Quy trình trích xuất được thiết kế để tự động hóa quy trình trích xuất dữ liệu từ các gói hồ sơ một cách hiệu quả. Quy trình làm việc bao gồm các bước sau:
Tác nhân giám sát giao nhiệm vụ trích xuất cho tác nhân trích xuất dữ liệu.
Tác nhân trích xuất dữ liệu gọi Amazon Bedrock Data Automation để phân tích cú pháp và trích xuất chi tiết người nộp đơn từ các gói hồ sơ.
Thông tin hồ sơ được trích xuất được lưu trữ trong bucket Amazon Simple Storage Service (Amazon S3) của các tài liệu đã trích xuất.
Phản hồi gọi Amazon Bedrock Data Automation được gửi lại cho tác nhân trích xuất.
Tác nhân xác thực (Validation agent)
Tác nhân xác thực đối chiếu dữ liệu được trích xuất với các tài nguyên bên ngoài như hồ sơ thuế của IRS và báo cáo tín dụng, gắn cờ các điểm không nhất quán để xem xét. Nó gắn cờ những điểm không nhất quán như các tệp PDF đã bị chỉnh sửa, điểm tín dụng thấp, đồng thời tính toán tỷ lệ nợ trên thu nhập (DTI), giới hạn cho vay trên giá trị (LTV) và kiểm tra sự ổn định việc làm.

Sơ đồ sau đây minh họa quy trình làm việc. Quy trình bao gồm các bước sau:
Tác nhân giám sát giao nhiệm vụ xác thực cho tác nhân xác thực.
Tác nhân xác thực truy xuất chi tiết người nộp đơn được lưu trữ trong bucket S3 của các tài liệu đã trích xuất.
Chi tiết người nộp đơn được đối chiếu với các tài nguyên của bên thứ ba, chẳng hạn như hồ sơ thuế và báo cáo tín dụng, để xác thực thông tin của người nộp đơn.
Chi tiết được xác thực của bên thứ ba được tác nhân xác thực sử dụng để tạo trạng thái.
Tác nhân xác thực gửi trạng thái xác thực cho tác nhân giám sát.
Tác nhân tuân thủ (Compliance agent)
Tác nhân tuân thủ xác minh rằng dữ liệu được trích xuất và xác thực tuân thủ các yêu cầu quy định, giảm nguy cơ vi phạm tuân thủ. Nó xác thực dựa trên các quy tắc cho vay. Ví dụ, các khoản vay chỉ được chấp thuận nếu tỷ lệ DTI của người vay dưới 43%, đảm bảo họ có thể quản lý các khoản thanh toán hàng tháng, hoặc các đơn có điểm tín dụng dưới 620 sẽ bị từ chối, trong khi điểm cao hơn đủ điều kiện nhận lãi suất tốt hơn.

Sơ đồ sau đây minh họa quy trình làm việc của tác nhân tuân thủ. Quy trình làm việc bao gồm các bước sau:
Tác nhân giám sát giao nhiệm vụ xác thực tuân thủ cho tác nhân tuân thủ.
Tác nhân tuân thủ truy xuất chi tiết người nộp đơn được lưu trữ trong bucket S3 của các tài liệu đã trích xuất.
Chi tiết người nộp đơn được xác thực dựa trên các quy tắc xử lý thế chấp.
Tác nhân tuân thủ tính toán tỷ lệ DTI của người nộp đơn, áp dụng chính sách của công ty và các quy tắc cho vay vào đơn.
Tác nhân tuân thủ sử dụng các chi tiết đã được xác thực để tạo trạng thái.
Tác nhân tuân thủ gửi trạng thái tuân thủ cho tác nhân giám sát.
Tác nhân thẩm định (Underwriting agent)
Tác nhân thẩm định tạo một tài liệu thẩm định để người thẩm định xem xét. Quy trình làm việc của tác nhân thẩm định hợp lý hóa quy trình xem xét và hoàn thiện các tài liệu thẩm định, như được hiển thị trong sơ đồ sau.

Quy trình làm việc bao gồm các bước sau:
Tác nhân giám sát giao nhiệm vụ thẩm định cho tác nhân thẩm định.
Tác nhân thẩm định xác minh thông tin và tạo một bản nháp của tài liệu thẩm định.
Tài liệu nháp được gửi cho người thẩm định để xem xét.
Các cập nhật từ người thẩm định được gửi lại cho tác nhân thẩm định.
Ma trận RACI
Sự hợp tác giữa các tác nhân thông minh và các chuyên gia con người là chìa khóa cho hiệu quả và trách nhiệm giải trình. Để minh họa điều này, chúng tôi đã tạo ra một ma trận RACI (Responsible, Accountable, Consulted, and Informed - Chịu trách nhiệm, Chịu trách nhiệm giải trình, Được tham vấn và Được thông báo) vạch ra cách trách nhiệm có thể được chia sẻ giữa các tác nhân do AI điều khiển và các vai trò của con người, chẳng hạn như nhân viên tuân thủ và nhân viên thẩm định. Bản đồ này đóng vai trò như một hướng dẫn khái niệm, đưa ra một cái nhìn thoáng qua về cách tự động hóa dựa trên tác nhân có thể nâng cao chuyên môn của con người, tối ưu hóa quy trình làm việc và cung cấp trách nhiệm giải trình rõ ràng. Việc triển khai trong thế giới thực sẽ khác nhau dựa trên cấu trúc và nhu cầu hoạt động riêng của một tổ chức.

Các thành phần của ma trận như sau:
R: Responsible (Thực hiện công việc)
A: Accountable (Sở hữu quyền phê duyệt và kết quả)
C: Consulted (Cung cấp ý kiến)
I: Informed (Được thông báo về tiến độ/trạng thái)
Kiến trúc tự động hóa IDP đầu cuối cho xử lý thế chấp
Sơ đồ kiến trúc sau đây minh họa các dịch vụ AWS cung cấp năng lượng cho giải pháp và phác thảo hành trình người dùng từ đầu đến cuối, cho thấy cách mỗi thành phần tương tác trong quy trình làm việc.

Trong Bước 1 và 2, quy trình bắt đầu khi người dùng truy cập giao diện người dùng web (web UI) trong trình duyệt của họ, với Amazon CloudFront duy trì việc phân phối nội dung có độ trễ thấp trên toàn thế giới. Trong Bước 3, Amazon Cognito xử lý xác thực người dùng, và AWS WAF cung cấp bảo mật chống lại các mối đe dọa độc hại. Bước 4 và 5 cho thấy người dùng đã được xác thực tương tác với ứng dụng web để tải lên tài liệu cần thiết lên Amazon S3. Các tài liệu được tải lên trong Amazon S3 kích hoạt Amazon EventBridge, khởi tạo quy trình làm việc Amazon Bedrock Data Automation để xử lý tài liệu và trích xuất thông tin.
Trong Bước 6, AWS AppSync quản lý các tương tác của người dùng, cho phép giao tiếp thời gian thực với AWS Lambda và Amazon DynamoDB để lưu trữ và truy xuất dữ liệu. Bước 7, 8, và 9 trình bày cách khung công tác đa tác nhân Amazon Bedrock đi vào hoạt động, nơi tác nhân giám sát điều phối quy trình làm việc giữa các tác nhân AI chuyên biệt. Tác nhân xác minh xác minh các tài liệu được tải lên, quản lý việc thu thập dữ liệu và sử dụng các nhóm hành động (action groups) để tính toán tỷ lệ DTI và tạo ra một bản tóm tắt, được lưu trữ trong Amazon S3.
Bước 10 cho thấy tác nhân xác thực (trợ lý môi giới) đánh giá đơn đăng ký dựa trên các tiêu chí kinh doanh được xác định trước và tự động tạo ra một thư chấp thuận sơ bộ, hợp lý hóa việc xử lý khoản vay với sự can thiệp tối thiểu của con người. Trong suốt quy trình làm việc ở Bước 11, Amazon CloudWatch cung cấp khả năng giám sát, ghi nhật ký toàn diện và hiển thị thời gian thực vào tất cả các thành phần hệ thống, duy trì độ tin cậy hoạt động và theo dõi hiệu suất.
Kiến trúc hoàn toàn tự động và dựa trên tác nhân (agentic) này tăng cường quy trình xử lý thế chấp bằng cách cải thiện hiệu quả, giảm lỗi và tăng tốc độ phê duyệt, cuối cùng mang lại trải nghiệm cho vay nhanh hơn, thông minh hơn và có khả năng mở rộng hơn.
Các yếu tố cần chuẩn bị trước
Bạn cần phải có một tài khoản AWS và  AWS Identity and Access Management (IAM) role và user với các quyền để tạo và quản lý các tài nguyên và thành phần cần thiết cho giải pháp này. Nếu bạn chưa có tài khoản AWS, hãy xem (How do I create and activate a new Amazon Web Services account?)
Triển khai giải pháp
Để bắt đầu, clone GitHub repository và làm theo hướng dẫn trong tệp README để triển khai giải pháp bằng cách sử dụng AWS CloudFormation. Các bước triển khai cung cấp hướng dẫn rõ ràng về cách xây dựng và triển khai giải pháp. Sau khi giải pháp được triển khai, bạn có thể tiếp tục với các hướng dẫn sau:
Sau khi bạn cung cấp tất cả các stack, hãy điều hướng đến stack AutoLoanAPPwebsitewafstackXXXXX trên bảng điều khiển AWS CloudFormation.
Trên tab Outputs, tìm điểm cuối CloudFront cho giao diện người dùng ứng dụng.

Bạn cũng có thể lấy endpoint dùng AWS Command Line Interface (AWS CLI) và dùng lệnh sau:


aws cloudformation describe-stacks
--stack-name $(aws cloudformation list-stacks
--stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE | jq -r '.StackSummaries[] | select(.StackName | startswith("AutoLoanAPPwebsitewafstack")) | .StackName')
--query 'Stacks[0].Outputs[?OutputKey==`configwebsitedistributiondomain`].OutputValue'
--output text
Mở https://<domain_name>.cloudfront.net trên một browser mới

Tạo một Amazon Cognito user để truy cập tới ứng dụng
Đăng nhập dùng Amazon Cognito email và password credentials để truy cập ứng dụng.
Giám sát và khắc phục sự cố
Xem xét các phương pháp hay nhất sau:
Giám sát trạng thái tạo và cập nhật stack bằng bảng điều khiển AWS CloudFormation hoặc AWS CLI.
Giám sát các chỉ số gọi mô hình Amazon Bedrock bằng CloudWatch:
Yêu cầu và độ trễ InvokeModel
Các ngoại lệ điều tiết
Lỗi 4xx và 5xx
Kiểm tra Amazon CloudTrail để biết các lệnh gọi API và lỗi.
Kiểm tra CloudWatch để biết các lỗi và nhật ký dành riêng cho giải pháp.
Dọn dẹp
Để tránh phát sinh thêm chi phí sau khi thử nghiệm giải pháp này, hãy hoàn thành các bước sau:
Xóa các stack liên quan khỏi bảng điều khiển AWS CloudFormation.
Xác minh các bucket S3 trống trước khi xóa chúng.
Kết luận
Giải pháp mẫu ứng dụng cho vay tự động minh họa cách bạn có thể sử dụng Amazon Bedrock Agents và Amazon Bedrock Data Automation để chuyển đổi các quy trình xử lý hồ sơ vay thế chấp. Ngoài việc xử lý thế chấp, bạn có thể điều chỉnh giải pháp này để hợp lý hóa việc xử lý yêu cầu bồi thường hoặc giải quyết các kịch bản xử lý tài liệu phức tạp khác. Bằng cách sử dụng tự động hóa thông minh, giải pháp này giúp giảm đáng kể nỗ lực thủ công, rút ngắn thời gian xử lý và đẩy nhanh quá trình ra quyết định. Tự động hóa các quy trình làm việc phức tạp này giúp các tổ chức đạt được hiệu quả hoạt động cao hơn, duy trì sự tuân thủ nhất quán với các quy định đang phát triển và mang lại trải nghiệm khách hàng đặc biệt.
Giải pháp mẫu được cung cấp dưới dạng mã nguồn mở—hãy sử dụng nó làm điểm khởi đầu cho giải pháp của riêng bạn và giúp chúng tôi làm cho nó tốt hơn bằng cách đóng góp lại các bản sửa lỗi và tính năng bằng cách sử dụng các yêu cầu kéo trên GitHub. Duyệt đến kho lưu trữ GitHub để khám phá mã, nhấp vào "watch" để được thông báo về các bản phát hành mới và kiểm tra README để biết các bản cập nhật tài liệu mới nhất.
Là các bước tiếp theo, chúng tôi khuyên bạn nên đánh giá các quy trình xử lý tài liệu hiện tại của mình để xác định các lĩnh vực phù hợp để tự động hóa bằng cách sử dụng Amazon Bedrock Agents và Amazon Bedrock Data Automation. Để được hỗ trợ chuyên môn, AWS Professional Services và các Đối tác AWS khác luôn sẵn sàng trợ giúp. Chúng tôi rất muốn nghe từ bạn. Hãy cho chúng tôi biết suy nghĩ của bạn trong phần bình luận hoặc sử dụng diễn đàn "issues" trong kho lưu trữ.


