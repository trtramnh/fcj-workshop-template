---
title: "Cấu hình Security Groups"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---


<!-- TODO: ChÃ¨n áº£nh chá»¥p mÃ n hÃ¬nh giao diá»‡n cáº¥u hÃ¬nh Security Group vÃ o Ä‘Ã¢y -->
![Security Groups](/images/5-Workshop/placeholder-sg.png)
Security Group (SG) hoạt động như một tường lửa ảo cấp độ Interface. Kiến trúc chuẩn quy định các tầng ứng dụng chỉ được phép giao tiếp với tầng liền kề nó. 

Vào **EC2 ➔ Security Groups ➔ Create security group** và cấu hình chặt chẽ theo 3 lớp sau:

### 1. ALB Security Group (`snaptics-alb-sg`)
Đây là cửa ngõ duy nhất hứng traffic từ người dùng.
- **VPC:** `snaptics-vpc`
- **Inbound Rules:**
  - Type: `HTTP` (Port 80) | Source: `0.0.0.0/0`
  - Type: `HTTPS` (Port 443) | Source: `0.0.0.0/0`
- **Outbound Rules:** Mặc định Allow All.

### 2. ECS Backend Security Group (`snaptics-ecs-sg`)
Bảo vệ các container .NET. Nó không được phép nhận request trực tiếp từ Public IP.
- **VPC:** `snaptics-vpc`
- **Inbound Rules:**
  - Type: `Custom TCP` | Port Range: `8080` (hoặc `80` tùy vào port bạn EXPOSE trong Dockerfile).
  - Source: Chọn Custom, gõ vào `snaptics-alb-sg` và chọn ID của nhóm ALB ở trên.
  *(Nghĩa là: Chỉ có Load Balancer mới được quyền gọi vào Container)*.
- **Outbound Rules:** Mặc định Allow All.

### 3. RDS Security Group (`snaptics-rds-sg`)
Bảo vệ cơ sở dữ liệu SQL Server.
- **VPC:** `snaptics-vpc`
- **Inbound Rules:**
  - Type: `MS SQL` | Port Range: `1433`
  - Source: Chọn Custom, gõ vào `snaptics-ecs-sg` và chọn ID của nhóm ECS ở trên.
  *(Nghĩa là: Chỉ có các Container .NET của Snaptics mới được quyền gọi vào Database)*.

> [!CAUTION]
> Tuyệt đối không mở Inbound Port 1433 cho `0.0.0.0/0`. Các hacker trên thế giới liên tục rà quét port này để thực thi mã độc mã hóa tống tiền (Ransomware) vào SQL Server.
