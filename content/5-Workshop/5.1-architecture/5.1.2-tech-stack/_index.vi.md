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

- **Amazon ECS Fargate:** Đóng gói ứng dụng .NET Backend và AI Worker thành các Container (Docker) chạy trên ECS Cluster. Fargate tự động vận hành container trong các Private Subnet trải rộng trên 02 Availability Zone mà không cần quản lý máy chủ EC2.
- **Amazon Aurora & RDS (Primary / Standby):** Dịch vụ Cơ sở dữ liệu quan hệ hoàn toàn tự động. Hỗ trợ cơ chế Multi-AZ (Primary ở AZ 2 và Standby ở AZ 1) với tính năng nhân bản liên tục, tự động sao lưu và khôi phục khi có sự cố.
- **Amazon S3 (Simple Storage Service):** Kho lưu trữ Object Storage dùng chứa ảnh hóa đơn và tệp dữ liệu. Kết nối với ECS qua **S3 Gateway Endpoint** trực tiếp trong VPC nhằm tối ưu hóa chi phí đường truyền và tăng cường bảo mật.
- **AWS Amplify:** Nền tảng hosting và tự động hóa build/deploy Frontend SPA từ GitHub Repo.
- **Amazon CloudFront & Route 53:** Route 53 quản lý tên miền; CloudFront đóng vai trò CDN điều phối lưu lượng đến Frontend Amplify và chuyển tiếp API Request qua Internet Gateway đến ALB.

### Lớp Điều phối & Tích hợp (Messaging & Integration)

- **Amazon SQS (`snaptics-ai-queue`) & DLQ:** Hàng đợi thông điệp đóng vai trò làm bộ đệm (Buffer) xử lý tác vụ OCR/AI bất đồng bộ, kết hợp Dead Letter Queue (DLQ) giữ lại các message bị lỗi để kiểm tra lại.
- **Amazon SNS (Simple Notification Service):** Dịch vụ phát thông báo cảnh báo sự cố vận hành và trạng thái hệ thống.
- **AWS Secrets Manager:** Quản lý và cung cấp an toàn các thông tin cấu hình nhạy cảm (RDS Connection String, Gemini API Key, Azure Credentials, JWT Secret) cho Fargate Tasks.

### Lớp Trí tuệ Nhân tạo (AI & OCR)

- **Google Gemini API (Vision):** Xử lý ảnh và hỏi đáp tài chính thông minh (Ví dụ: "Tháng này tôi tiêu nhiều nhất vào khoản nào?").
- **Azure Document Intelligence:** Công cụ OCR cực kỳ mạnh mẽ của Microsoft, bóc tách chính xác các trường dữ liệu tĩnh từ hóa đơn (Tên cửa hàng, Mã số thuế, Thành tiền, Ngày giờ) một cách hoàn toàn tự động.
