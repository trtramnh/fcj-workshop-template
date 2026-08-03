---
title: "Worklog Tuần 4"
date: 2026-06-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4

* Họp nhóm thống nhất chọn đề tài dự án **Snaptics** - Nền tảng quản lý chi tiêu cá nhân/gia đình và quét hóa đơn thông minh.
* Phân tích bài toán quản lý tài chính thực tế và xác định phạm vi tính năng MVP cho 2 nhóm người dùng: User và Admin.
* Nhận vai trò **Frontend Developer – UI/UX Web Design** và lập kế hoạch thiết kế giao diện ứng dụng.
* Tìm kiếm các mẫu giao diện tham khảo, xây dựng Sitemap hệ thống và phác thảo User Flow chính.
* Thiết kế bộ Wireframe ban đầu cho các màn hình cốt lõi và thống nhất phong cách thiết kế UI (Design System tokens).
* Tìm hiểu các dịch vụ AWS hỗ trợ vận hành dự án: Elastic Load Balancer (ALB), Auto Scaling, CloudWatch, AWS Budgets, Route 53 và AWS CLI.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Họp nhóm 4 thành viên thảo luận và thống nhất lựa chọn đề tài dự án Snaptics.<br>- Phân tích bài toán quản lý chi tiêu: nhập liệu thủ công tốn thời gian, dữ liệu bị phân tán, khó kiểm soát ngân sách gia đình.<br>- Phân định phạm vi bài toán MVP dành cho 2 vai trò duy nhất: User và Admin.<br>- Phân chia nhiệm vụ: Tôi đảm nhận vai trò Frontend Developer & UI/UX Designer. | 01/06/2026 | 01/06/2026 | [Proposal Snaptics](2-Proposal/) |
| 3 | - Nghiên cứu các ứng dụng quản lý tài chính hàng đầu để học hỏi tư duy UI/UX.<br>- Xây dựng sơ đồ cấu trúc trang (Sitemap) chi tiết cho cả khu vực User (Dashboard, Transactions, Scan, Wallet, Budget, Analysis, AI Chat, Notifications, Support, Settings) và Admin (User Mgmt, Ticket Mgmt, Notification Mgmt, Hangfire Jobs, Settings).<br>- Phác thảo sơ đồ luồng người dùng (User Flow) cho tính năng quét hóa đơn và tạo giao dịch. | 02/06/2026 | 02/06/2026 | [UI/UX Design Patterns](https://refactoringui.com/) |
| 4 | - Thiết kế bộ khung bố cục (Wireframe) phác thảo cho các màn hình chính (Dashboard, Transaction List, Receipt Scan, Wallet Management, Admin Panel).<br>- Thống nhất bộ Design System ban đầu: bảng màu chủ đạo (Primary Dark Blue, Emerald Accent), Typography (Inter Font), khoảng cách, kiểu dáng Button & Card. | 03/06/2026 | 03/06/2026 | [Modern Web Layouts](https://uxdesign.cc/) |
| 5 | - Nghiên cứu dịch vụ hạ tầng AWS: Application Load Balancer (ALB) phân phối lưu lượng và Auto Scaling Group (ASG) tự động điều chỉnh số lượng server theo tải.<br>- Phân tích mô hình cân bằng tải và tính sẵn sàng cao cho ứng dụng web Snaptics. | 04/06/2026 | 04/06/2026 | [AWS ALB Guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)<br>[Auto Scaling Guide](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html) |
| 6 | - Tìm hiểu các dịch vụ giám sát và quản lý chi phí: Amazon CloudWatch, AWS Budgets và Route 53.<br>- Cấu hình thử nghiệm AWS Budgets cảnh báo khi chi phí vượt ngưỡng.<br>- Viết câu lệnh AWS CLI kiểm tra thông tin tài nguyên hệ thống và tổng kết kết quả công việc tuần 4. | 05/06/2026 | 05/06/2026 | [Amazon CloudWatch Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)<br>[AWS Budgets Guide](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |

### Kết quả đạt me tuần 4

* Chính thức thống nhất đề tài Snaptics và định hình bài toán quản lý tài chính cá nhân/gia đình thông minh.
* Phân định rõ ràng mục tiêu tính năng MVP cho 2 nhóm người dùng: User và Admin.
* Nhận vai trò Frontend Developer – UI/UX Web Design và hoàn thành lập kế hoạch thiết kế giao diện.
* Hoàn thành tài liệu Sitemap chi tiết định hình toàn bộ cấu trúc màn hình ứng dụng Snaptics.
* Thiết kế thành công sơ đồ User Flow chuẩn hóa cho các tác vụ chính (quét hóa đơn, quản lý chi tiêu).
* Hoàn thành bộ Wireframe ban đầu phác thảo bố cục giao diện trực quan.
* Thống nhất bộ nhận diện UI Design System (Color Tokens, Typography, Buttons, Form Inputs).
* Nắm vững nguyên lý hoạt động của các dịch vụ AWS mở rộng: ALB, Auto Scaling, CloudWatch, Budgets, Route 53 và AWS CLI.
