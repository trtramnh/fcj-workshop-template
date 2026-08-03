---
title: "Worklog Tuần 11"
date: 2026-07-27
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11

* Soạn thảo và hoàn thiện bài Blog 1: "Kiến trúc Website Thương mại Điện tử có khả năng mở rộng trên AWS" (Phân tích ECS Fargate, ALB, CloudFront, Aurora Serverless v2, ElastiCache).
* Soạn thảo và hoàn thiện bài Blog 2: "Hành trang AWS & Tư duy Cloud-Native từ kỳ thực tập thực tế" (Chia sẻ trải nghiệm sử dụng Amazon S3, AWSSDK.S3, Decoupling, IAM Least Privilege, Pre-signed URL, CORS).
* Soạn thảo và hoàn thiện bài Blog 3: "AWS đã nâng cấp Amazon Cognito như thế nào mà người dùng gần như không hề nhận ra?" (Phân tích kiến trúc Zero Downtime Migration, Dual-write, Anti-entropy validation).
* Xuất bản trọn vẹn cả bản Tiếng Việt và Tiếng Anh của 3 bài Blog vào hệ thống Hugo (`content/3-BlogsPosted/`) kèm theo hình ảnh minh họa chất lượng cao.
* Rà soát và cập nhật liên kết chéo (Cross-links) giữa các mục Worklog, Proposal, Workshop và Blogs Posted trong báo cáo.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Viết nội dung Blog 1 chuyên đề Kiến trúc Thương mại Điện tử mở rộng trên AWS.<br>- Phân tích chi tiết luồng xử lý request: Route 53 -> CloudFront -> AWS WAF -> ALB -> ECS Fargate -> ElastiCache / Aurora Serverless v2.<br>- Phân tích cơ chế giám sát và tự động phát cảnh báo qua CloudWatch Alarms và Amazon SNS. | 27/07/2026 | 27/07/2026 | [Blog 1 Overview](3-BlogsPosted/3.1-Blog1/)<br>[AWS Scalable Web Guidance](https://docs.aws.amazon.com/solutions/building-a-containerized-and-scalable-web-application-on-aws/) |
| Thứ 3 | - Viết nội dung Blog 2 tổng kết kinh nghiệm thực tế khi tương tác với Amazon S3 trong dự án.<br>- Trình bày tư duy phân tách (Decoupling) tính toán và lưu trữ, ứng dụng thư viện AWSSDK.S3 trong C#/.NET.<br>- Chia sẻ bài học thực chiến về quản trị IAM Least Privilege, bảo mật Secret Key qua Environment Variables, sinh Pre-signed URL và xử lý sự cố CORS. | 28/07/2026 | 28/07/2026 | [Blog 2 Overview](3-BlogsPosted/3.2-Blog2/)<br>[Amazon S3 Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) |
| Thứ 4 | - Viết nội dung Blog 3 phân tích quy trình hiện đại hóa hạ tầng Amazon Cognito của AWS.<br>- Phân tích các tính năng mới: High-throughput performance, Customer-managed keys (KMS) và Multi-Region replication.<br>- Phân tích phương pháp nâng cấp hệ thống quy mô lớn với mục tiêu Zero Downtime: Shadow mode validation, Dual-write architecture và Anti-entropy validation. | 29/07/2026 | 29/07/2026 | [Blog 3 Overview](3-BlogsPosted/3.3-Blog3/)<br>[AWS Cognito Infrastructure Upgrade Blog](https://aws.amazon.com/blogs/security/amazon-cognito-unlocks-advanced-capabilities-with-next-generation-infrastructure/) |
| Thứ 5 | - Định dạng Markdown và nhúng sơ đồ kiến trúc cho cả 3 bài Blog trên giao diện báo cáo Hugo.<br>- Kiểm tra đường dẫn hình ảnh trong `static/images/` đảm bảo hiển thị sắc nét trên cả giao diện Tiếng Việt và Tiếng Anh.<br>- Rà soát thẻ Hashtag và các đường link tài liệu tham khảo chính thức của AWS cho từng bài blog. | 30/07/2026 | 30/07/2026 | [Blogs Posted Directory](3-BlogsPosted/)<br>[Hugo Content Management](https://gohugo.io/content-management/formats/) |
| Thứ 6 | - **Rà soát & Đồng bộ hệ thống:**<br>- Thực hiện kiểm tra đồng bộ thông tin ngày tháng (`date: 2026-07-27`) và thứ tự weight cho 3 bài blog.<br>- Kiểm tra các liên kết nội bộ giữa Worklog tuần 10-11 tới các bài Blog Posted tương ứng.<br>- Lưu lại toàn bộ mã nguồn bài viết và sẵn sàng cho giai đoạn tổng kết kỳ thực tập. | 31/07/2026 | 31/07/2026 | [Blogs Posted Directory](3-BlogsPosted/)<br>[Hugo Shortcodes](https://gohugo.io/content-management/shortcodes/) |

### Kết quả đạt được tuần 11

* Hoàn thành xuất bản Blog 1 chia sẻ kiến trúc Web Thương mại Điện tử mở rộng, có khả năng chịu tải lớn và tự động giám sát qua CloudWatch/SNS.
* Hoàn thành xuất bản Blog 2 đúc kết kinh nghiệm thực chiến với Amazon S3, tư duy Decoupling kiến trúc, lập trình AWSSDK.S3, bảo mật IAM và kỹ thuật Pre-signed URL.
* Hoàn thành xuất bản Blog 3 phân tích quy trình Zero Downtime Migration nâng cấp hạ tầng Amazon Cognito của AWS với các kỹ thuật Shadow Mode và Dual-write.
* Đồng bộ hoàn toàn cả 2 phiên bản ngôn ngữ (Tiếng Việt `_index.vi.md` và Tiếng Anh `_index.md`) cho cả 3 bài Blog trong `content/3-BlogsPosted/`.
* Tích hợp đầy đủ hình ảnh sơ đồ kiến trúc minh họa chất lượng cao trong thư mục `images/` cho từng bài viết.
* Tối ưu hóa liên kết nội bộ và định dạng Markdown chuẩn xác trên nền tảng báo cáo thực tập Hugo.
