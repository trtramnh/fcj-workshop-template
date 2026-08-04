---
title: "Thiết lập Môi trường Local"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---


Bạn cần cài đặt các công cụ sau trên máy tính của mình. 

### 1. Cài đặt AWS CLI v2
Đây là công cụ bắt buộc để tương tác với tài nguyên AWS từ terminal, đặc biệt là khi chúng ta chạy file script `deploy.ps1`.
- Tải từ trang chủ: [AWS CLI Version 2](https://aws.amazon.com/cli/)
- Sau khi cài đặt, mở Terminal/PowerShell và cấu hình thông tin bảo mật:

```bash
aws configure
```
Hệ thống sẽ yêu cầu bạn nhập 4 thông số:
- **AWS Access Key ID:** `<YOUR_ACCESS_KEY_ID>` (Lấy từ IAM User của bạn)
- **AWS Secret Access Key:** `<YOUR_SECRET_ACCESS_KEY>`
- **Default region name:** `ap-southeast-1` (Nên chọn khu vực phù hợp để có được độ trễ thấp nhất)
- **Default output format:** `json`

### 2. Cài đặt Docker
Snaptics chạy trên kiến trúc Serverless Container (ECS Fargate), vì vậy bạn phải đóng gói ứng dụng thành một Docker Image trước khi đẩy lên AWS.
- Tải **Docker Desktop** (cho Windows/Mac) tại [docker.com](https://www.docker.com/products/docker-desktop/).
- Kiểm tra xem Docker đã chạy thành công chưa:
```bash
docker info
```

### 3. Cài đặt .NET 8 / 9 SDK
Mặc dù bạn có thể build bên trong Docker, việc có sẵn SDK trên máy giúp bạn có thể chạy Entity Framework Migrations, test ứng dụng local, hoặc thiết lập các file cấu hình `appsettings.json`.
- Tải tại [dotnet.microsoft.com](https://dotnet.microsoft.com/download).

### 4. Chuẩn bị AI API Keys
Do Snaptics phụ thuộc vào AI để trích xuất hóa đơn, hãy đảm bảo bạn đã tạo tài khoản và lấy được 2 API keys sau:
1. **Google Gemini API Key**: Lấy tại [Google AI Studio](https://aistudio.google.com/app/apikey).
2. **Azure Document Intelligence**: Đăng nhập [Azure Portal](https://portal.azure.com/), tạo resource Document Intelligence và copy lấy **Endpoint** và **Key 1**.
