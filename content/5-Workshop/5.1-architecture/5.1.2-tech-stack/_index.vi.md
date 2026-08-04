---
title: "Tech Stack & AWS Services"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.1.2. </b> "
---


Để vận hành một hệ thống tài chính mượt mà như Snaptics, chúng ta sử dụng một hệ sinh thái kết hợp giữa .NET Core và hệ sinh thái Managed Services của AWS. Dưới đây là bảng phân tích vai trò của từng công nghệ trong dự án:

### Lớp Lõi Ứng Dụng (Application Core)

- **C# .NET 8 / 9:** Framework mạnh mẽ được sử dụng để xây dựng Web API. Hiệu năng cao, bảo mật chặt chẽ nhờ định kiểu tĩnh, rất phù hợp cho ứng dụng tài chính.
- **Entity Framework Core (EF Core):** ORM chính để giao tiếp với cơ sở dữ liệu SQL Server. Quản lý việc ánh xạ các Model (như `Transactions`, `Invoices`) thành các bảng trong CSDL.
- **Hangfire:** Thư viện chạy tác vụ nền (Background Jobs). Dùng để lên lịch các tác vụ như tính toán chi tiêu cuối tháng, rollover ngân sách sang tháng mới.
- **SignalR:** Công nghệ WebSockets của .NET giúp đẩy thông báo (Push Notifications) thời gian thực về thiết bị di động khi hóa đơn được quét xong.

### Lớp Cơ sở Hạ tầng (AWS Infrastructure)

- **Amazon ECS Fargate:** Đóng gói toàn bộ ứng dụng .NET bên trên thành 1 container (Docker). Fargate giúp chạy container này mà không cần tốn công cấu hình hay bảo trì máy chủ EC2 gốc. Tự động mở rộng (Scale out) khi lượng request tăng đột biến.
- **Amazon RDS (SQL Server):** Dịch vụ Database được quản lý hoàn toàn. Hỗ trợ tự động sao lưu (Auto-backup), patch lỗi bảo mật và Multi-AZ để đảm bảo chống lỗi phần cứng.
- **Amazon S3 (Simple Storage Service):** Kho lưu trữ Object Storage giá rẻ và bền bỉ vô tận, dùng để chứa mọi file ảnh, PDF biên lai của người dùng trước khi gửi đi phân tích OCR.

### Lớp Điều phối & Tích hợp (Messaging & Integration)

- **Amazon SNS (Simple Notification Service):** Chủ yếu dùng để bắn các tín hiệu cảnh báo khẩn cấp hoặc gửi luồng event cho nhiều subcriber cùng lúc.
- **Amazon SQS (Simple Queue Service):** Hàng đợi message. Đóng vai trò làm bộ đệm (Buffer) khi lượng hóa đơn tải lên quá lớn, giúp hệ thống không bị nghẽn (Decoupling).
- **AWS Parameter Store:** Nơi lưu trữ an toàn các tham số cấu hình nhạy cảm. Ứng dụng .NET sẽ tự động load cấu hình từ đây lúc startup để tránh bị lộ Secret Key ra mã nguồn.

### Lớp Trí tuệ Nhân tạo (AI & OCR)

- **Google Gemini API (Vision):** Xử lý ảnh và hỏi đáp tài chính thông minh (Ví dụ: "Tháng này tôi tiêu nhiều nhất vào khoản nào?").
- **Azure Document Intelligence:** Công cụ OCR cực kỳ mạnh mẽ của Microsoft, bóc tách chính xác các trường dữ liệu tĩnh từ hóa đơn (Tên cửa hàng, Mã số thuế, Thành tiền, Ngày giờ) một cách hoàn toàn tự động.
