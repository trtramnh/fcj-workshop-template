---
title: "Worklog Tuần 11"
date: 2026-07-20
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11

* Hoàn thiện cơ chế **Angular CanActivate AdminGuard** phía Frontend Client (chỉ cho phép tài khoản có vai trò Admin truy cập).
* Xây dựng bộ khung Layout Admin chuyên biệt (Admin Sidebar với phong cách sậm màu chuyên nghiệp, Admin Header).
* Xây dựng trang Quản lý Người dùng (Admin User Management UI) hỗ trợ tra cứu, lọc và khóa/mở khóa tài khoản.
* Xây dựng trang Quản lý Yêu cầu Hỗ trợ (Admin Support Ticket Management UI) phục vụ tiếp nhận và phản hồi khách hàng.
* Thiết kế trang Quản lý Thông báo Hệ thống (Admin Notification UI) và Trang Cấu hình Hệ thống (System Settings UI).
* Xây dựng giao diện Quản lý Hangfire Job (Hangfire Job Management UI) với các nút kích hoạt chạy thử (`Trigger Now`) và cài đặt lịch chạy.
* Thiết kế Trang 404 Not Found, Trang Bảo trì Hệ thống (System Maintenance Page) và rà soát phân quyền Admin/User.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Cấu hình bộ lọc điều hướng AdminGuard trên Angular Router phía Client.<br>- Xây dựng bộ khung Admin Layout chuyên biệt: Admin Sidebar hiển thị các menu quản trị (User, Support Tickets, System Notifications, Hangfire Jobs, System Config) và Admin Top Header.<br>- Phân định rõ hai vai trò người dùng duy nhất trên hệ thống: Admin và User. | 20/07/2026 | 20/07/2026 | [Admin Dashboard Layouts](https://dribbble.com/) |
| 3 | - Thiết kế giao diện trang Quản lý Người dùng (Admin User Management Page).<br>- Xây dựng Bảng danh sách người dùng với các thông tin: ID, Avatar, Họ tên, Email, Vai trò, Trạng thái (Hoạt động / Bị khóa), Ngày tham gia và Thao tác.<br>- Phát triển công cụ tìm kiếm theo Tên/Email, bộ lọc trạng thái và Modal xem chi tiết/Khóa tài khoản người dùng. | 21/07/2026 | 21/07/2026 | [User Management Table Design](https://uxdesign.cc/) |
| 4 | - Thiết kế giao diện trang Quản lý Yêu cầu Hỗ trợ (Admin Support Ticket Management Page).<br>- Xây dựng bộ lọc Ticket theo trạng thái (Open, In Progress, Resolved, Closed) và mức độ ưu tiên.<br>- Thiết kế màn hình xem nội dung Ticket và ô nhập phản hồi của Admin gửi lại cho User kèm tính năng thay đổi trạng thái xử lý. | 22/07/2026 | 22/07/2026 | [Helpdesk Admin UI](https://uicoach.io/) |
| 5 | - Thiết kế trang Quản lý Thông báo Hệ thống (tạo và phát thông báo tới toàn bộ người dùng) và trang Cấu hình Hệ thống.<br>- Xây dựng giao diện Quản lý Hangfire Job (Hangfire Job Management UI):<br>&emsp; + Hiển thị danh sách các Background Jobs (Job OCR, Job Cảnh báo ngân sách, Job Tổng hợp báo cáo).<br>&emsp; + Thêm nút kích hoạt chạy thử tức thời (`Trigger Now`).<br>&emsp; + Thiết kế phần cấu hình thời gian lịch chạy (Schedule Picker UI). | 23/07/2026 | 23/07/2026 | [Hangfire Dashboard UI](https://www.hangfire.io/) |
| 6 | - Thiết kế màn hình 404 Not Found (Trang không tìm thấy) và Trang Bảo trì Hệ thống (System Maintenance Page).<br>- Kiểm tra lại toàn bộ cơ chế phân quyền giữa Admin và User, đảm bảo không có quyền truy cập trái phép.<br>- Khắc phục các lỗi hiển thị vỡ khung trên giao diện Admin và tổng kết tuần 11. | 24/07/2026 | 24/07/2026 | [404 & Maintenance Page Design](https://collectui.com/) |

### Kết quả đạt được tuần 11

* Hoàn thành bộ khung giao diện Admin Panel hiện đại, tách biệt hoàn toàn với giao diện người dùng thường.
* Thiết lập Angular AdminGuard bảo vệ an toàn các tuyến đường điều hướng quản trị ở phía Frontend Client.
* Hoàn thành trang Quản lý Người dùng hỗ trợ tìm kiếm, lọc và thực hiện các thao tác quản trị tài khoản tiện lợi.
* Phát triển thành công trang Quản lý Support Ticket giúp Admin tiếp nhận và phản hồi người dùng nhanh chóng.
* Thiết kế thành công trang Quản lý Thông báo Hệ thống và trang Cấu hình chung.
* Xây dựng giao diện Quản lý Hangfire Job sinh động với nút bấm Trigger Now và bộ cài đặt lịch chạy trực quan.
* Hoàn thành thiết kế Trang 404 Not Found và Trang Bảo trì Hệ thống ấn tượng.
* Đảm bảo tính tuân thủ tuyệt đối về phân quyền duy nhất 2 vai trò: Admin và User.
* Sửa toàn bộ các lỗi vỡ khung giao diện Admin trên các màn hình có độ phân giải nhỏ.
