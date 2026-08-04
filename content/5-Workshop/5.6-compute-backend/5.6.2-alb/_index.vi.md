---
title: "Application Load Balancer"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---


Trước khi cấu hình ECS, chúng ta cần một Load Balancer đứng ở ngoài Public Subnet để nhận request từ người dùng và phân tải ngược lại cho các container Fargate nằm trong mạng Private.

### 1. Tạo Target Group
Target Group là danh sách các IP của container mà ALB sẽ chia traffic tới.
- Vào **EC2 ➔ Target Groups ➔ Create target group**.
- **Choose a target type:** Bắt buộc chọn **IP addresses** (vì Fargate sử dụng mạng `awsvpc`, mỗi container sẽ có một IP riêng).
- **Target group name:** `snaptics-tg`
- **Protocol / Port:** `HTTP` / `8080` (Cổng của ứng dụng .NET).
- **VPC:** `snaptics-vpc`
- Ở bước "Register targets", cứ để trống và bấm **Create** (ECS sẽ tự động đăng ký IP vào Target Group này sau).

### 2. Khởi tạo Load Balancer
- Vào **EC2 ➔ Load Balancers ➔ Create Load Balancer**.
- Chọn **Application Load Balancer (ALB)**.
- **Name:** `snaptics-alb`
- **Scheme:** `Internet-facing` (Để ứng dụng kết nối được từ bên ngoài).
- **Network mapping:** Chọn `snaptics-vpc`. Tick chọn cả 2 **Public Subnets** (`snaptics-public-1a` và `1b`).
- **Security groups:** Bỏ default SG, chọn nhóm `snaptics-alb-sg` đã tạo ở bài trước.
- **Listeners and routing:** Ở cổng `HTTP:80`, phần Default action chọn Forward tới `snaptics-tg`.
- Bấm **Create**.

> [!TIP]
> Để API chạy an toàn với HTTPS, bạn cần mua Domain và dùng dịch vụ AWS Certificate Manager (ACM) để tạo chứng chỉ SSL miễn phí, sau đó thêm một Listener ở Port 443 trên ALB.
