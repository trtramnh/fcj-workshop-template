---
title: "Worklog Tuần 9"
date: 2026-07-13
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9

* Phân tích chi tiết yêu cầu bài toán thực tế dự án nhóm "IoT Weather Platform for Lab Research" dành cho phòng nghiên cứu ITea Lab.
* Xác định mục tiêu và phạm vi hệ thống (Thu nhập dữ liệu từ 5 trạm thời tiết Raspberry Pi + ESP32, khả năng mở rộng 15 trạm, truyền gói tin MQTT).
* Lựa chọn và thiết kế hệ thống dịch vụ AWS Serverless hợp nhất: AWS IoT Core, AWS Lambda, Amazon S3 Data Lake, AWS Glue Crawlers/ETL, API Gateway, AWS Amplify và Amazon Cognito.
* Vẽ sơ đồ kiến trúc chi tiết cho 2 thành phần chính: Edge Architecture (Thiết bị biên) và Platform Architecture (Nền tảng Đám mây).
* Tính toán chi tiết ngân sách vận hành bằng công cụ AWS Pricing Calculator và lập ma trận đánh giá rủi ro cho dự án Proposal.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Họp nhóm thống nhất nội dung Bản đề xuất (Proposal) dự án "IoT Weather Platform for Lab Research".<br>- Phân tích thực trạng: các trạm thời tiết cũ thu thập thủ công, thiếu hệ thống lưu trữ tập trung và phân tích thời gian thực.<br>- Xác định người dùng mục tiêu (5 nghiên cứu viên ITea Lab) và bài toán cần giải quyết. | 13/07/2026 | 13/07/2026 | [Proposal Document](2-Proposal/)<br>[AWS IoT Core Overview](https://docs.aws.amazon.com/iot/latest/developerguide/what-is-aws-iot.html) |
| Thứ 3 | - Nghiên cứu thiết kế luồng dữ liệu tiếp nhận (Ingestion) và xử lý (ETL).<br>- Lựa chọn AWS IoT Core làm cổng nhận tin nhắn MQTT từ thiết bị biên Raspberry Pi.<br>- Phân tích mô hình S3 Data Lake: Bucket 1 chứa dữ liệu thô (Raw Data Lake), Bucket 2 chứa dữ liệu đã làm sạch và chuyển đổi (Analytical Data). | 14/07/2026 | 14/07/2026 | [AWS IoT Rules](https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html)<br>[Building Data Lakes on AWS](https://aws.amazon.com/solutions/implementations/data-lake-solution/) |
| Thứ 4 | - Tìm hiểu giải pháp phân tích dữ liệu tự động bằng AWS Glue (Crawlers lập chỉ mục dữ liệu S3 & ETL Jobs chuyển đổi định dạng).<br>- Phân tích vai trò của AWS Amplify lưu trữ ứng dụng fullstack Next.js và Amazon Cognito quản lý xác thực người dùng an toàn.<br>- Lựa chọn công cụ AWS CDK/SDK để lập trình hạ tầng theo dạng mã nguồn. | 15/07/2026 | 15/07/2026 | [AWS Glue Developer Guide](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html)<br>[Amazon Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html) |
| Thứ 5 | - **Thực hành thiết kế sơ đồ kiến trúc:**<br>- Vẽ sơ đồ Edge Architecture mô tả luồng cảm biến ESP32 -> Raspberry Pi (Docker) -> MQTT qua Wi-Fi.<br>- Vẽ sơ đồ Platform Architecture thể hiện kết nối giữa IoT Core -> S3 -> Glue -> Lambda -> API Gateway -> Amplify Next.js.<br>- Hoàn thiện ma trận đánh giá rủi ro (mất mạng, hỏng cảm biến, vượt ngân sách) và phương án dự phòng. | 16/07/2026 | 16/07/2026 | [AWS Architecture Icons](https://aws.amazon.com/architecture/icons/)<br>[Proposal Section 3](2-Proposal/#3-kiến-trúc-giải-pháp) |
| Thứ 6 | - **Tính toán ngân sách & Hoàn thiện Proposal:**<br>- Sử dụng AWS Pricing Calculator tính toán chi tiết từng dịch vụ (Lambda $0, S3 $0.15, Amplify $0.35, Glue $0.09, IoT Core $0.08).<br>- Xác nhận tổng chi phí hạ tầng cloud tối ưu: **0.70 USD/tháng** (~8.40 USD/năm).<br>- Đưa toàn bộ nội dung Proposal lên thư mục `content/2-Proposal/` của báo cáo Hugo và kiểm tra hiển thị. | 17/07/2026 | 17/07/2026 | [AWS Pricing Calculator](https://calculator.aws/)<br>[Proposal Budget Section](2-Proposal/#6-ước-tính-ngân-sách) |

### Kết quả đạt được tuần 9

* Thống nhất thành công đề xuất dự án "IoT Weather Platform for Lab Research", giải quyết triệt để bài toán thu thập và phân tích dữ liệu thời tiết thủ công.
* Thiết kế thành công kiến trúc Đám mây Serverless hoàn chỉnh kết hợp giữa AWS IoT Core, S3 Data Lake, AWS Glue, Lambda, API Gateway, Amplify và Cognito.
* Hoàn thành bộ sơ đồ kiến trúc đạt chuẩn bao gồm Edge Architecture (máy biên Raspberry Pi/ESP32) và Platform Architecture (nền tảng Serverless AWS).
* Lập ma trận đánh giá rủi ro bài bản (mất kết nối mạng, hỏng phần cứng) và đề xuất giải pháp lưu trữ đệm (buffering) bằng Docker trên thiết bị biên.
* Ước tính ngân sách chính xác thông qua AWS Pricing Calculator với chi phí cực kỳ tối ưu (0.70 USD/tháng), giúp dự án đạt hiệu quả kinh tế (ROI) cao.
* Cập nhật đầy đủ bản Proposal Tiếng Việt và Tiếng Anh lên cấu trúc Hugo (`content/2-Proposal/_index.vi.md` và `_index.md`), sẵn sàng chuyển sang giai đoạn triển khai.
