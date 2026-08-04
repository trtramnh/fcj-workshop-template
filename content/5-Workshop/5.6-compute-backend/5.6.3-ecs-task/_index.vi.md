---
title: "ECS Cluster & Task Definition"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---


<!-- TODO: ChÃ¨n áº£nh mÃ n hÃ¬nh Amazon ECS Cluster (cho tháº¥y Task Fargate Ä‘ang cháº¡y) vÃ o Ä‘Ã¢y -->
![ECS Fargate](/images/5-Workshop/placeholder-ecs.png)
Sau khi ứng dụng đã thành Image trên ECR và Load Balancer đã sẵn sàng chia tải, chúng ta sẽ tạo máy chủ Serverless chạy Image đó.

### 1. Tạo Cluster
- Mở **Amazon ECS ➔ Clusters ➔ Create Cluster**.
- **Cluster name:** `Snaptics-Cluster`.
- **Infrastructure:** Chọn AWS Fargate (Serverless).

### 2. Khai báo Task Definition
Task Definition giống như "Bản thiết kế" (Blueprint) chỉ dẫn cho ECS biết phải chạy Image nào, cần bao nhiêu RAM và vCPU.
- Mở **Task definitions ➔ Create new task definition**.
- **Task definition family:** `snaptics-api-task`.
- **Infrastructure requirements:** Fargate (Linux/X86_64).
  - CPU: `1 vCPU`
  - Memory: `2 GB` (Đủ cho .NET + Hangfire chạy mượt mà).
  - **Task role:** Chọn `snaptics-ecs-task-role` (Cho phép code trong container gọi S3, SQS).
  - **Task execution role:** Chọn `ecsTaskExecutionRole`.
- **Container - 1:**
  - Name: `snaptics-app`
  - Image URI: Dán link ảnh trên ECR của bạn vào (VD: `<ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/snaptics-api:latest`).
  - Container port: `8080`.
- Bấm **Create**.

### 3. Triển khai Service
Service là trình điều phối đảm bảo ứng dụng của bạn luôn có X bản sao (Task) đang chạy. Nó cũng kết nối ứng dụng với Load Balancer.
- Vào `Snaptics-Cluster` ➔ Tab **Services** ➔ **Create**.
- **Environment:** Launch type ➔ Fargate.
- **Deployment configuration:**
  - Application type: `Service`
  - Family: `snaptics-api-task`
  - Service name: `snaptics-backend-service`
  - Desired tasks: `2` (Chạy 2 container để dự phòng lỗi).
- **Networking:**
  - VPC: `snaptics-vpc`
  - Subnets: Tick chọn **2 Private Subnets** (`snaptics-private-1a` và `1b`).
  - Security group: Chọn **Use an existing** ➔ `snaptics-ecs-sg`.
  - **Public IP:** Tắt (Turn off). Vì Fargate nằm ở Private nên không được phép có IP Public.
- **Load balancing:**
  - Type: Application Load Balancer
  - Container: `snaptics-app 8080:8080`
  - Chọn Use an existing load balancer ➔ `snaptics-alb`.
  - Target group: `snaptics-tg`.
- Bấm **Create**.

Hãy kiên nhẫn đợi khoảng 2-3 phút, khi tab `Tasks` báo có 2 task chuyển sang trạng thái **RUNNING**, bạn có thể lấy tên miền (DNS Name) của ALB dán vào trình duyệt và chiêm ngưỡng thành quả!
