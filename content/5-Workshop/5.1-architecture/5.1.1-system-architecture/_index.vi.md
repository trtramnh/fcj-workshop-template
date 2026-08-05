---
title: "Sơ đồ Kiến trúc Hệ thống"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1.1. </b> "
---


<img src="/images/2-Proposal/snaptics_architecture.png" alt="Snaptics AWS Architecture" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />

Thiết kế mạng Multi-Stack của Snaptics tuân thủ nghiêm ngặt nguyên tắc bảo mật Cloud-Native: cô lập hoàn toàn tầng Public Subnet và Private Subnet trên 2 Availability Zone (AZ), điều phối lưu lượng qua Amazon Route 53, CloudFront, Application Load Balancer (ALB) và tự động hóa CI/CD qua GitHub Actions.



### Phân tích Luồng Hoạt động (Data Flow từ 1 đến 10)

1. **[1] User Request:** Người dùng truy cập hệ thống; **Amazon Route 53** phân giải tên miền chỉ định đến điểm phân phối **Amazon CloudFront**.
2. **[2] Content Delivery & Edge Routing:** CloudFront điều phối lưu lượng giao diện người dùng (Frontend Web) được lưu trữ trên **AWS Amplify**, đồng thời chuyển hướng các request API qua **Internet Gateway**.
3. **[3] Ingress Load Balancing:** Request API đi qua Internet Gateway đến **Application Load Balancer (ALB)** đặt trong Public Subnet.
4. **[4] Compute Dispatching:** ALB định tuyến request API đến các container backend thuộc **ECS Cluster (Fargate Tasks)** đang vận hành an toàn trong Private Subnet trải rộng trên 02 Availability Zone.
5. **[5] Object Storage Access:** Các Fargate Task đọc/ghi ảnh hóa đơn và tệp tĩnh trực tiếp với **Amazon S3 Bucket** thông qua **Gateway Endpoint** trong VPC, không đi qua Internet để tối ưu bảo mật và chi phí.
6. **[6] Database Persistence:** Fargate Task thực hiện xử lý dữ liệu nghiệp vụ và lưu trữ dữ liệu vào cơ sở dữ liệu **Aurora & RDS (Primary / Standby)** được cấu hình đồng bộ Multi-AZ.
7. **[7] Asynchronous Queueing & DLQ:** Các tác vụ nặng (như bóc tách OCR/AI) được đẩy vào **Amazon SQS (`snaptics-ai-queue`)**. Các Worker Task trên ECS bốc message ra xử lý ngầm. Nếu gặp sự cố vượt số lần retry, message được chuyển sang **Dead Letter Queue (DLQ)** để kiểm tra và xử lý lại.
8. **[8] Outbound Egress:** Khi cần gọi API bên ngoài, Fargate Task gửi request truy cập ra ngoài thông qua **NAT Gateway** trong Public Subnet.
9. **[9] Gateway Egress:** Traffic từ NAT Gateway đi qua **Internet Gateway** để kết nối ra mạng Internet công cộng.
10. **[10] External AI Integration:** Request được gửi đến các **External AI APIs** (như Azure Document Intelligence bóc tách hóa đơn và Google Gemini API cho AI Insights). Kết quả trả về được lưu trữ ngược lại vào Aurora & RDS.

---

### Các Thành Phần Phụ Trợ & CI/CD

* **Management & Observability:**
  * **Amazon CloudWatch:** Thu thập log, metric và thiết lập cảnh báo hoạt động của hệ thống.
  * **AWS Secrets Manager:** Lưu trữ và cung cấp an toàn các thông tin nhạy cảm (API Key, JWT Secret, Connection String).
  * **AWS Budgets:** Theo dõi và gửi cảnh báo khi chi phí hạ tầng chạm ngưỡng.
  * **Amazon SNS (Simple Notification Service):** Gửi thông báo cảnh báo vận hành và sự cố hệ thống.

* **CI/CD Pipeline (GitHub Actions):**
  * Developer push code lên **GitHub Repo**, kích hoạt **GitHub Actions**.
  * **Auto Build & Deploy:** Tự động build và triển khai Frontend lên **AWS Amplify**.
  * **Build & Push Docker Images:** Đóng gói Docker Container và đẩy image lên **Elastic Container Registry (ECR)**.
  * **Update Service:** Cập nhật service trên **ECS Cluster**, các Fargate Task tự động kéo image mới nhất từ ECR (**Pull Image**).

