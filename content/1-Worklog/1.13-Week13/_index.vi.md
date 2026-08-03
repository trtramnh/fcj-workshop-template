---
title: "Worklog Tuần 13"
date: 2026-08-03
weight: 13
chapter: false
pre: " <b> 1.13. </b> "
---

### Mục tiêu tuần 13

* Tiến hành kiểm thử toàn bộ luồng người dùng (End-to-End User Flow testing) từ giao diện User tới Admin Panel.
* Khắc phục tất cả các lỗi vặt giao diện Frontend (UI/UX polishing) và tối ưu hóa mã nguồn ứng dụng Angular.
* Kiểm tra và cấu hình các biến môi trường Production (`environment.prod.ts`) sẵn sàng cho việc build sản phẩm.
* Thực hiện biên dịch bản Frontend Production tĩnh thành công bằng Angular CLI (`ng build --configuration production`).
* Hỗ trợ triển khai ứng dụng Frontend lên dịch vụ **AWS Amplify** kết hợp quy trình CI/CD tự động từ GitHub.
* Tìm hiểu và hỗ trợ cấu hình phân phối nội dung với Amazon CloudFront, Amazon Route 53 (tên miền) và chứng chỉ HTTPS.
* Thực hành bài lab **Workshop 5.6**: Tiến hành dọn dẹp sạch sẽ tài nguyên thử nghiệm (VPC Endpoints, EC2 test instances, IAM policies, S3 test buckets) tránh phát sinh chi phí thừa trên AWS.
* Chụp ảnh màn hình giao diện thực tế, hoàn thiện hồ sơ tài liệu báo cáo thực tập, chuẩn bị kịch bản demo sản phẩm và nộp chính thức dự án Snaptics vào ngày **07/08/2026**.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tiến hành End-to-End Testing toàn bộ luồng người dùng trên ứng dụng Frontend Angular:<br>&emsp; + Luồng Đăng nhập, Đăng ký, Phân quyền Client.<br>&emsp; + Luồng Dashboard, Giao dịch, Ví cá nhân/gia đình, Ngân sách.<br>&emsp; + Luồng Quét hóa đơn OCR, Thông báo, Chatbot AI Insight, Support Ticket.<br>&emsp; + Luồng Quản trị Admin (User Mgmt, Ticket Mgmt, Notification Mgmt, Hangfire Jobs).<br>- Rà soát và tinh chỉnh các lỗi vặt UI về khoảng cách, hiệu ứng hover và icon hiển thị. | 03/08/2026 | 03/08/2026 | [Software Testing Best Practices](https://developer.mozilla.org/) |
| 3 | - Rà soát tối ưu hóa mã nguồn Angular, dọn dẹp các module/component không dùng.<br>- Kiểm tra chính xác các biến môi trường Production (`src/environments/environment.prod.ts` cho API Endpoint URL, S3 Bucket base URL).<br>- Chạy lệnh `ng build --configuration production` biên dịch bản tĩnh Production và xác nhận quá trình build hoàn tất thành công 100% không phát sinh lỗi. | 04/08/2026 | 04/08/2026 | [Angular Deployment Guide](https://angular.io/guide/deployment) |
| 4 | - Hỗ trợ triển khai ứng dụng web Frontend lên dịch vụ **AWS Amplify** hosting.<br>- Kết nối GitHub Repository dự án với AWS Amplify Console.<br>- Cấu hình kịch bản build tự động (`amplify.yml` cho Angular build) và kiểm thử quy trình CI/CD tự động build/deploy sản phẩm mỗi khi push code lên nhánh chính. | 05/08/2026 | 05/08/2026 | [AWS Amplify Hosting User Guide](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html) |
| 5 | - **Thực hành Workshop 5 (Phần 6 - Dọn dẹp tài nguyên):**<br>&emsp; + Tiến hành dọn dẹp toàn bộ tài nguyên lab Workshop theo hướng dẫn ([Workshop Clean-up](5-Workshop/5.6-Cleanup/)).<br>&emsp; + Xóa Gateway VPC Endpoint, Interface VPC Endpoint, các EC2 test instances, S3 test buckets và giải phóng Elastic IP.<br>&emsp; + Rà soát trên AWS Cost Explorer đảm bảo không còn tài nguyên rác chạy ngầm gây phát sinh chi phí.<br>- Tìm hiểu cấu hình tên miền trên Amazon Route 53, CloudFront CDN và chứng chỉ HTTPS; Chụp ảnh giao diện ứng dụng. | 06/08/2026 | 06/08/2026 | [Workshop Clean-up](5-Workshop/5.6-Cleanup/)<br>[Amazon Route 53 Guide](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) |
| 6 | - **Hoàn thiện & Nộp Báo cáo Thực tập:**<br>- Chuẩn bị tài liệu báo cáo thực tập và kịch bản Demo sản phẩm phục vụ buổi tổng kết.<br>- Tổng kết lại toàn bộ các kiến thức kỹ thuật về Frontend Angular Web Design, UI/UX, bài thực hành Workshop 5 VPC Endpoints và các dịch vụ đám mây AWS đã tích lũy trong 13 tuần.<br>- Hoàn thiện hồ sơ báo cáo thực tập và **chính thức nộp Project Snaptics** khép lại chương trình thực tập FCJ Workforce vào ngày **07/08/2026**. | 07/08/2026 | 07/08/2026 | [FCJ Workforce Regulations](https://hcm-rules.awsfcaj.com/1-regulations/) |

### Kết quả đạt được tuần 13

* Thực hiện kiểm thử End-to-End thành công, đảm bảo toàn bộ luồng người dùng từ User đến Admin vận hành trơn tru.
* Tối ưu hóa dung lượng ứng dụng Angular, nâng cao tốc độ tải trang và trải nghiệm người dùng.
* Thực hiện biên dịch bản Build Angular Production thành công 100% không gặp bất kỳ lỗi syntax hay thiếu tài nguyên.
* Triển khai thành công ứng dụng Frontend lên dịch vụ AWS Amplify kết hợp quy trình CI/CD tự động hóa từ GitHub.
* Thực hành dọn dẹp thành công 100% tài nguyên bài lab Workshop 5 (Clean-up), giải phóng VPC Endpoints và EC2 instances thử nghiệm, đảm bảo tối ưu chi phí AWS.
* Hỗ trợ thiết lập thành công phân phối CDN qua CloudFront, quản lý DNS trên Route 53 và bảo mật HTTPS.
* Thu thập bộ ảnh chụp màn hình thực tế giao diện ứng dụng Snaptics chất lượng cao cho báo cáo.
* Chuẩn bị đầy đủ kịch bản Demo sản phẩm và nội dung bảo vệ tổng kết.
* Đúc kết được nhiều kinh nghiệm thực chiến giá trị về Frontend Angular Development, bài thực hành Workshop VPC Endpoints và các dịch vụ đám mây AWS.
* Hoàn thành và nộp chính thức Báo cáo thực tập & Project Snaptics đúng thời hạn vào ngày **07/08/2026**.
