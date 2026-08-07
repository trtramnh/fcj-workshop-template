---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


Chào mừng bạn đến với hướng dẫn triển khai toàn diện dự án **Snaptics** - một hệ thống quản lý tài chính thông minh được xây dựng bằng **.NET 10** và Frontend Angular/Amplify.

Trong workshop này, bạn sẽ học cách triển khai một môi trường Production thực thụ, có tính khả dụng cao và bảo mật tuyệt đối trên AWS. Kiến trúc đã được nâng cấp mạnh mẽ từ mức cơ bản lên chuẩn **Enterprise (Doanh nghiệp)**, tuân thủ nghiêm ngặt khung AWS Well-Architected Framework.

### Những Điểm Nâng cấp Đáng giá trong Kiến trúc này:
- **Phân phối Toàn cầu & Bảo mật:** Triển khai **Route 53** (Quản lý DNS) và **Application Load Balancer** để phân tải tốc độ cao.
- **Hosting Frontend:** Tự động hóa hoàn toàn việc hosting ứng dụng Frontend thông qua **AWS Amplify**.
- **Serverless Compute:** Vận hành Backend .NET Core trên **Amazon ECS Fargate** thông qua Application Load Balancer (ALB) trong mạng Private.
- **Database Chuẩn Doanh nghiệp:** Chuyển đổi từ SQL Server thông thường sang **Amazon RDS for SQL Server** với cơ chế sao chép Primary/Standby (Multi-AZ) để đảm bảo High Availability.
- **Bảo mật Storage & Bí mật:** Loại bỏ việc gọi file S3 qua NAT Gateway đắt đỏ, thay bằng **VPC Gateway Endpoint** siêu bảo mật. Quản lý chuỗi kết nối an toàn với **AWS Systems Manager Parameter Store**.
- **Hàng đợi AI Bền bỉ:** Nâng cấp hàng đợi Amazon SQS `snaptics-ai-queue` bằng cách gắn thêm **Dead Letter Queue (DLQ)**, giúp hứng và xử lý lại các message lỗi khi gọi AI.
- **Tự động hóa CI/CD 100%:** Xóa bỏ việc chạy script thủ công ở máy cá nhân. Chúng ta sẽ dùng **GitHub Actions** để tự động Build Docker, đẩy lên ECR và cập nhật cụm ECS mỗi khi có code mới.
- **Giám sát (Observability):** Tập trung log và cảnh báo hệ thống thông qua **Amazon CloudWatch**, **SNS**, và **AWS Budgets**.

---

### Cấu trúc Workshop

Để việc theo dõi menu bên trái được gọn gàng nhất nhưng nội dung bên trong vẫn giữ nguyên độ sâu và chi tiết khổng lồ, workshop được chia thành 9 phân hệ chính. Vui lòng làm theo đúng thứ tự:

1. **Tổng quan & Kiến trúc:** Phân tích chuyên sâu sơ đồ hệ thống Enterprise và luồng dữ liệu.
2. **Chuẩn bị (Prerequisites):** Thiết lập tài khoản GitHub, IAM Users và lấy API Key cho AI (Gemini / Azure).
3. **Mạng & Bảo mật:** Khởi tạo Multi-Tier VPC, cấu hình Route 53, ALB và VPC Endpoints.
4. **Database, Storage & Secrets:** Triển khai RDS SQL Server, S3 Bucket và khởi tạo kho khóa Parameter Store.
5. **AI & Tác vụ nền (Messaging):** Cấu hình SQS `snaptics-ai-queue` kèm DLQ, tích hợp các API Trí tuệ nhân tạo.
6. **Compute & Backend (ECS):** Đóng gói Docker và điều phối các task Fargate ẩn sau lớp ALB.
7. **CI/CD Pipeline (GitHub Actions):** Viết script YAML để tự động hóa toàn bộ quy trình Deploy cho cả Frontend lẫn Backend.
8. **Kiểm thử Toàn hệ thống (E2E Testing):** Xác thực luồng API và kết nối WebSocket (SignalR) xuyên qua ALB.
9. **Dọn dẹp Tài nguyên:** Bước cực kỳ quan trọng để xóa sạch hạ tầng, tránh bị AWS trừ tiền thẻ tín dụng.

> [!NOTE]  
> Hướng dẫn này được viết cực kỳ chi tiết từ A đến Z, bạn chỉ cần đọc chậm rãi và copy/paste cấu hình chính xác. Hãy chuẩn bị một ly cà phê và bắt tay vào xây dựng hệ thống tỷ đô của riêng bạn nào!