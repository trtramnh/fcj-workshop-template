---
title: "Compute & Load Balancing (ECS)"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---


Hệ thống Backend của Snaptics hoạt động trên nền tảng Serverless Containers. Ta sẽ bọc ứng dụng `.NET` thành một gói Docker và quăng nó lên chạy trên cụm Amazon ECS Fargate.

## 1. Application Load Balancer

Vì các Server Fargate nằm nấp ở mạng Private, ta phải xây một con ALB đứng ngoài mạng Public để nhận request từ Internet và chia đều tải vào trong.

### A. Khởi tạo Target Group
- Vào **EC2 ➔ Target Groups ➔ Create target group**.
- **Target type:** Bắt buộc chọn **IP addresses** (Vì Fargate dùng mạng ảo `awsvpc`).
- **Target group name:** `snaptics-ecs-tg`.
- **Protocol / Port:** `HTTP / 8080`.
- **VPC:** `snaptics-vpc`.
- Ở bước chọn Target, cứ bỏ trống (Lát nữa ECS sẽ tự động bơm IP vào đây) và bấm Create.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/alb_tg_create.png" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/alb_tg_create_2.png" >
  </div>

### B. Khởi tạo ALB
- Vào **EC2 ➔ Load Balancers ➔ Create Load Balancer ➔ Application Load Balancer**.
- **Name:** `snaptics-alb`.
- **Scheme:** Chọn **Internet-facing** để Load Balancer có thể giao tiếp với Internet.
- **Network mapping:** Chọn `snaptics-vpc` và tick vào 2 mạng **Public Subnets**.
- **Security groups:** Chọn `snaptics-alb-sg`.
- **Listeners and routing:** Mở cổng HTTP (80) và trỏ luồng Forward vào `snaptics-ecs-tg`.
- Bấm Create.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/alb_create_1.png" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/alb_create_1.2.png" >
  </div>

## 2. Kho chứa Docker (Amazon ECR)

Trước khi cấu hình ECS, ta cần một kho chứa an toàn để GitHub Actions bắn file Docker Image vào.
- Vào **Amazon ECR ➔ Repositories ➔ Create repository**.
- **Visibility settings:** Private (Kín).
- **Repository name:** `snaptics-api`.
- Bấm Create. Copy lại chuỗi **URI** (Ví dụ: `123456789.dkr.ecr.ap-southeast-1.amazonaws.com/snaptics-api`).

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/ecr_create_1.png" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/ecr_create_1.2.png" >
  </div>

*(Lưu ý: Ta sẽ không push image bằng tay bằng dòng lệnh ở đây. Việc này sẽ do GitHub Actions lo trọn gói ở phần tiếp theo).*

## 3. Khởi tạo ECS Cluster & Task Definition

### A. Tạo Cluster
- Vào **Amazon ECS ➔ Clusters ➔ Create Cluster**.
- **Name:** `Snaptics-Cluster`.
- **Infrastructure:** AWS Fargate.
- Hãy BẬT tính năng **Container Insights**. Tính năng này sẽ đẩy log cực kỳ chi tiết của hệ thống về màn hình giám sát CloudWatch (Như trong sơ đồ kiến trúc có vẽ mục Observability).

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/ecs_cluster_create.png" >
  </div>

### B. Tạo Bản thiết kế (Task Definition)
- Vào **Task definitions ➔ Create new task definition**.
- **Family:** `snaptics-api-task`.
- **Infrastructure:** Fargate (Linux/X86_64).
- **CPU:** `0.5 vCPU`.
- **Memory:** `2 GB` (Đủ bộ nhớ cho cả luồng `.NET` và hệ thống nền `Hangfire` chạy mượt mà).
- **Task role:** Chọn `snaptics-ecs-task-role` (Giúp Code lấy được mật khẩu DB).
- **Task execution role:** Chọn `ecsTaskExecutionRole`.
- **Container - 1:**
  - Name: `snaptics-app`
  - Image URI: Dán chuỗi URI của ECR bạn vừa copy vào. *(Đừng lo nếu nó báo lỗi không tìm thấy image, lát GitHub Actions bơm code vào là nó tự chạy).*
  - Container port: `8080`.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.6-compute-backend/task_definition_create.png" >
  </div>

### C. Khởi chạy Service
- Vào cụm `Snaptics-Cluster` ➔ Tab **Services** ➔ **Create**.
- **Launch type:** Fargate.
- **Service name:** `snaptics-backend-service`.
- **Desired tasks:** `2` (Chạy song song 2 máy chủ ảo ở 2 mạng Private khác nhau để đảm bảo High Availability).
- **Networking:** Bắt buộc chọn 2 mạng **Private Subnets**. Tắt **OFF** cái Public IP đi (Server xịn là không được có IP Public). Gắn tường lửa `snaptics-ecs-sg`.
- **Load balancing:** Gắn vào cái ALB `snaptics-alb` và Target Group `snaptics-ecs-tg` vừa tạo ở bước 1.
- Bấm Create.

Lúc này Service sẽ cố gắng khởi động nhưng báo lỗi vì kho ECR đang trống không. Hãy chuyển ngay sang mục Kế tiếp để thiết lập đường ống CI/CD siêu cấp vũ trụ của GitHub Actions!

