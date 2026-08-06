---
title: "Tổng quan & Kiến trúc"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---


Trước khi bắt tay vào cấu hình thực tế trên AWS Console, việc thấu hiểu tường tận bản vẽ kiến trúc là bắt buộc. Sơ đồ Enterprise này áp dụng hàng loạt các tiêu chuẩn (Best Practices) của AWS về Bảo mật, Tính sẵn sàng cao (High Availability) và Tối ưu chi phí mạng.

## 1. Sơ đồ Kiến trúc Hệ thống Enterprise

![Snaptics System Architecture](/fcj-workshop-template/images/5-Workshop/5.1-Workshop-overview/snaptics-architecture.png)

### Phân tích Luồng Dữ Liệu Chuyên sâu 

Hãy nhìn kỹ vào các vòng tròn số màu đen trên sơ đồ. Chúng thể hiện vòng đời của một Request từ người dùng:

1. **Phân giải DNS (Route 53):** Khi người dùng truy cập ứng dụng, request sẽ chạm tới **Amazon Route 53**. Route 53 định tuyến các request của giao diện Frontend vào **AWS Amplify** và các request gọi API vào **Application Load Balancer (ALB)**.
2. **Xâm nhập VPC (IGW tới ALB):** Từ Route 53, luồng mạng đi xuyên qua **Internet Gateway**, tiến vào **Application Load Balancer (ALB)** đang đứng gác ở `Public Subnet`.
3. **Tầng Compute (ECS Fargate):** ALB đóng vai trò phân tải, đẩy request vào các container `.NET` đang chạy trên **ECS Fargate**. Các server này được giấu kín đáo và an toàn tuyệt đối bên trong `Private Subnet`.
4. **Lưu trữ An toàn (VPC Gateway Endpoint):** Khi Code .NET cần lưu file ảnh hóa đơn lên **S3 Bucket**, nó KHÔNG đi đường vòng ra Internet. Nhờ có **Gateway Endpoint**, dữ liệu được bắn trực tiếp từ mạng nội bộ VPC sang S3, giúp bảo mật tuyệt đối và loại bỏ hoàn toàn phí băng thông của NAT Gateway!
5. **Lưu trữ CSDL (SQL Server):** Dữ liệu giao dịch được ghi vào cụm **Amazon RDS for SQL Server**. Cụm này chạy chế độ **Primary/Standby** trải dài trên 2 Availability Zones. Nếu server chính sập, server phụ lập tức lên thay mà không gây sập hệ thống (High Availability).
6. **Xử lý Bất đồng bộ (SQS):** Các tác vụ nặng như gọi AI sẽ được ném vào hàng đợi `snaptics-ai-queue`. Đặc biệt, nếu tác vụ bị lỗi quá nhiều lần, nó sẽ bị tống vào **Dead Letter Queue (DLQ)** để chờ Admin vào xử lý thủ công, đảm bảo không nghẽn hệ thống.
7. **Định tuyến NAT Gateway:** Đối với các tác vụ thực sự cần kết nối ra Internet bên ngoài, ECS Container (ở mạng Private) sẽ phải đi qua cổng **NAT Gateway** (ở mạng Public).
8. **Lối ra Internet:** NAT Gateway chuyển tiếp luồng mạng tới Internet Gateway.
9. **Tích hợp AI Ngoại vi:** Request chính thức rời khỏi AWS Cloud, kết nối đến **External AI APIs** (Google Gemini, Azure Document Intelligence) để đọc hóa đơn và phân tích tài chính.

### Luồng Triển khai Tự động CI/CD 
- **Developer** viết code và Push lên **GitHub Repo**.
- **GitHub Actions** tự động bắt sự kiện.
- Nó tiến hành Build Docker Image và Push thẳng lên **Elastic Container Registry (ECR)**.
- Sau đó, GitHub Actions ra lệnh cho ECS Fargate cập nhật phiên bản mới (Zero downtime).
- Đối với Frontend, GitHub Actions tự động trigger **AWS Amplify** để deploy giao diện mới.

### Giám sát & Bảo mật 
- **CloudWatch** thu thập toàn bộ log sinh ra từ ECS và DB.
- **AWS Systems Manager Parameter Store** là két sắt mã hóa lưu giữ toàn bộ mật khẩu DB và API Key của AI.
- **SNS & AWS Budgets** phối hợp giám sát, tự động gửi Email cảnh báo nếu hóa đơn tiền điện toán AWS vượt quá ngân sách cho phép.

---

## 2. Tổng hợp Tech Stack

- **Frontend:** Angular/ Hosting tự động trên AWS Amplify.
- **Backend Core:** C# .NET 10 / Entity Framework Core / SignalR (WebSockets).
- **Database:** Amazon RDS for SQL Server & RDS Multi-AZ (Chống lỗi phần cứng).
- **Containerization:** Docker / Amazon Elastic Container Registry (ECR).
- **Compute:** Amazon ECS (Fargate) Serverless.
- **Networking:** Route 53, ALB, VPC Endpoints, NAT Gateway.
- **CI/CD:** Tự động hóa bằng GitHub Actions.
- **AI Services:** Google Gemini API, Azure Document Intelligence.

---

## 3. Ngân sách dự kiến 

Dưới đây là bảng ước tính chi phí chính xác cho môi trường Demo (1 tháng phát triển & demo), cũng như bảng tham khảo khi mở rộng lên môi trường Production Multi-AZ.

### 3.1. Bảng dự toán chi phí môi trường demo (1 tháng phát triển & demo)

| STT | Hạng mục dịch vụ | Cơ sở ước tính | Chi phí (USD) |
| :--- | :--- | :--- | :--- |
| 1 | **AWS Amplify & Route 53** | Build/hosting Frontend và 01 Hosted Zone | $4.50 |
| 2 | **Amazon S3** | Lưu khoảng 20 GB ảnh hóa đơn và request upload/download | $1.00 |
| 3 | **ECS Fargate - Backend & AI Worker** | Task cấu hình nhỏ, tổng khoảng 200-220 giờ chạy | $8.00 |
| 4 | **Application Load Balancer (ALB)** | Hoạt động trong giai đoạn triển khai và demo, lưu lượng thấp | $7.00 |
| 5 | **Amazon RDS for SQL Server** | SQL Server Express, Single-AZ, 20 GB | $20.00 |
| 6 | **NAT Gateway & Dữ liệu chuyển giao** | 01 NAT Gateway, bật giới hạn trong thời gian tích hợp | $13.00 |
| 7 | **Amazon SQS, SNS & ECR** | Queue OCR/AI, cảnh báo cơ bản và lưu Docker Image | $1.00 |
| 8 | **CloudWatch, Parameter Store & Budgets**| Log, metric, alarm, secret và cảnh báo ngân sách | $3.00 |
| 9 | **Azure Document Intelligence** | Khoảng 1.000 trang bằng prebuilt invoice model | $10.00 |
| 10 | **Gemini API** | Ước tính 1 triệu token input và 200.000 token output | $0.80 |
| | **Tổng chi phí dự kiến Demo** | | **~$68.30** |

### 3.2. Môi trường Production Multi-AZ (Tham khảo định hướng mở rộng)

| Hạng mục dịch vụ | Chi phí dự kiến / tháng |
| :--- | :--- |
| **ECS Fargate & Application Load Balancer (Auto Scaling)** | $60 - $150 USD |
| **SQL Server Primary/Standby (Multi-AZ)**| $150 - $300 USD |
| **Dual NAT Gateway & Data Transfer** | $70 - $120 USD |
| **S3, SQS, SNS, ECR & CloudWatch**| $20 - $60 USD |
| **External AI APIs (Azure Document Intelligence & Gemini)** | Phụ thuộc số lượng hóa đơn thực tế |
| **Tổng chi phí dự kiến Production** | **$300 - $600 USD / tháng** (chưa gồm AI API) |

> [!WARNING]
> **CẢNH BÁO TÀI CHÍNH CỰC KỲ QUAN TRỌNG:** Nếu bạn đang thực hành Workshop này trên tài khoản AWS cá nhân để học tập, **BẠN BẮT BUỘC** phải làm theo mục **5.9 Dọn dẹp Tài nguyên (Cleanup)** ngay sau khi test xong. Việc quên tắt SQL Server Multi-AZ và NAT Gateway có thể "đốt" sạch tiền trong thẻ tín dụng của bạn chỉ trong vài ngày!






