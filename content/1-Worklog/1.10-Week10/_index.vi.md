---
title: "Worklog Tuần 10"
date: 2026-07-13
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10

* Xây dựng giao diện Danh sách Thông báo (Notification Center Page & Header Dropdown Menu).
* Thiết kế biểu tượng Chuông thông báo (Notification Bell) kèm Badge đếm số thông báo chưa đọc.
* Phân loại và hiển thị giao diện cho từng nhóm thông báo: Quét hóa đơn, Cảnh báo ngân sách, Gợi ý AI, Lời mời ví gia đình.
* Xây dựng giao diện trang Yêu cầu Hỗ trợ (Support Ticket Page) với Form tạo Ticket và luồng thảo luận.
* Thực hành bài lab **Workshop 5.5**: Cấu hình VPC Endpoint IAM Policies để siết chặt an ninh và giới hạn quyền truy cập tài nguyên Amazon S3.
* Xây dựng trang Cài đặt Tài khoản Người dùng (User Account Settings UI) và tối ưu hóa trải nghiệm người dùng toàn ứng dụng.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thiết kế giao diện Notification Dropdown Menu trên Top Header và trang Danh sách thông báo tập trung (Notification Center Page).<br>- Xây dựng biểu tượng Chuông thông báo kèm Icon Badge màu đỏ đếm số lượng thông báo chưa đọc.<br>- Bổ sung các tính năng "Đánh dấu tất cả đã đọc" và "Lọc theo loại thông báo". | 13/07/2026 | 13/07/2026 | [Notification System UX](https://uxdesign.cc/) |
| 3 | - Phân loại thiết kế UI cho các mẫu thông báo ứng dụng:<br>&emsp; + Thông báo kết quả Quét hóa đơn hoàn tất.<br>&emsp; + Cảnh báo ngân sách chạm hoặc vượt hạn mức.<br>&emsp; + Gợi ý tài chính thông minh từ AI Insight.<br>&emsp; + Lời mời tham gia Ví gia đình từ thành viên khác.<br>- Bổ sung nút thao tác nhanh (Quick Action Button) trên card thông báo. | 14/07/2026 | 14/07/2026 | [In-App Notification Cards](https://material.io/) |
| 4 | - Thiết kế bố cục trang Yêu cầu Hỗ trợ (Support Ticket Page).<br>- Xây dựng Bảng danh sách Ticket đã gửi kèm phân loại trạng thái.<br>- Thiết kế Form Tạo Ticket hỗ trợ mới với các ô nhập liệu và validation phía Client. | 15/07/2026 | 15/07/2026 | [Helpdesk UI Patterns](https://dribbble.com/) |
| 5 | - **Thực hành Workshop 5 (Phần 5 - VPC Endpoint Policies):**<br>&emsp; + Tìm hiểu mô hình bảo mật phân lớp bằng VPC Endpoint IAM Policy ([Workshop VPC Endpoint Policies](5-Workshop/5.5-Policy/)).<br>&emsp; + Soạn thảo bản chính sách JSON Endpoint Policy gắn vào VPC Endpoint để chỉ cho phép truy cập duy nhất S3 Bucket của dự án Snaptics.<br>&emsp; + Chạy lệnh kiểm thử từ chối truy cập (Access Denied) khi truy vấn tới các S3 Buckets ngoài danh sách cho phép. | 16/07/2026 | 16/07/2026 | [Workshop VPC Endpoint Policies](5-Workshop/5.5-Policy/)<br>[VPC Endpoint Policy Reference](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html) |
| 6 | - Thiết kế màn hình Xem chi tiết Ticket (Ticket Detail & Discussion Thread UI).<br>- Xây dựng trang Cài đặt Tài khoản Người dùng (User Account Settings UI) hỗ trợ hồ sơ cá nhân, đổi mật khẩu và quản lý phiên đăng nhập.<br>- Kiểm tra lỗi form validation và tối ưu responsive tuần 10. | 17/07/2026 | 17/07/2026 | [Account Settings Layout](https://refactoringui.com/) |

### Kết quả đạt được tuần 10

* Hoàn thành trung tâm Thông báo (Notification Center) tích hợp mượt mà trên Header và trang riêng.
* Phân loại và thiết kế trực quan cho 4+ nhóm thông báo chính trong ứng dụng Snaptics.
* Hoàn thành giao diện trang Quản lý Yêu cầu Hỗ trợ (Support Ticket) chuyên nghiệp.
* Thực hành thành công bài lab Workshop 5.5: Soạn thảo và gắn thành công VPC Endpoint Policy, ngăn chặn truy cập trái phép tới các S3 Buckets không thuộc dự án.
* Thiết kế thành công luồng trao đổi thảo luận chi tiết trong Ticket giữa User và Đội ngũ Hỗ trợ.
* Hoàn thành trang Cài đặt Tài khoản Người dùng gọn gàng, bảo mật và loại bỏ các thành phần thừa.
* Đảm bảo tính nhất quán thẩm mỹ và khả năng phản hồi mượt mà trên mọi kích thước màn hình.
