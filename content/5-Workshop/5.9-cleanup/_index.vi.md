---
title: "Dọn dẹp Tài nguyên (Cleanup)"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---


Bước dọn dẹp là bước quan trọng nhất đối với sinh viên và những người học Cloud để tránh bị trừ tiền thẻ tín dụng. Hãy thực hiện tuần tự từ trên xuống dưới để đảm bảo các tài nguyên không bị khóa chéo lẫn nhau (VD: VPC không thể xóa nếu NAT Gateway vẫn còn sống).

> [!CAUTION]
> Tuyệt đối tuân thủ thứ tự dưới đây. Nếu gặp lỗi báo "Network Interface is in use", tức là bạn chưa xóa sạch tài nguyên dùng chung mạng.

### 1. Dọn dẹp ECS Fargate
- Mở **Amazon ECS ➔ Clusters ➔ `Snaptics-Cluster`**.
- Tick chọn `snaptics-backend-service` ➔ Bấm **Update**.
- Sửa mục `Desired tasks` về số **0** ➔ Bấm **Update**. (Hành động này ép AWS tắt máy chủ).
- Quay lại tab Services, đợi status của các Task về STOPPED. Tick lại vào Service ➔ **Delete**.
- Xóa luôn `Snaptics-Cluster`.

### 2. Dọn dẹp Load Balancer
- Mở **EC2 ➔ Load Balancers**.
- Tick chọn `snaptics-alb` ➔ Chọn Actions ➔ **Delete**.
- Sang tab **Target Groups**, xóa `snaptics-tg`.

### 3. Dọn dẹp Database RDS
- Mở **Amazon RDS ➔ Databases**.
- Tick chọn `snaptics-db` ➔ Actions ➔ **Delete**.
- **Quan trọng:** Bỏ tick dòng "Create final snapshot" (Tạo bản backup cuối), đồng ý xóa tự động backup, và gõ chữ `delete me` vào ô xác nhận. Bấm **Delete**.

### 4. Dọn dẹp NAT Gateway & Elastic IP (Ngốn tiền nhất)
- Mở **VPC ➔ NAT Gateways**.
- Tick chọn `snaptics-nat-gw` ➔ **Delete NAT gateway**.
- Chờ khoảng 3 phút cho chữ Deleted xuất hiện.
- Mở **VPC ➔ Elastic IPs**, tick chọn cái IP vừa tạo, Actions ➔ **Release Elastic IP addresses**. (Lỗi quên release Elastic IP sẽ bị AWS thu phí treo 1$/tháng).

### 5. Dọn dẹp S3, ECR, SQS, SNS, Parameter Store
- **S3:** Vào bucket `s3-bucket-snaptics`. Bạn phải bấm nút **Empty** (Xóa sạch file bên trong) thì hệ thống mới cho phép bạn bấm nút **Delete** bucket.
- **ECR:** Vào xóa repository chứa Docker image của bạn.
- **SQS/SNS:** Xóa queue `snaptics-main-queue` và topic `snaptics-alerts`.
- **Parameter Store:** Chọn từng dòng cấu hình và bấm xóa.

### 6. Xóa VPC Cuối cùng
- Mở **VPC ➔ Your VPCs**.
- Tick chọn `snaptics-vpc` ➔ Actions ➔ **Delete VPC**.
- Hành động này sẽ tự động thu dọn nốt toàn bộ Subnets, Route Tables, Internet Gateway và Security Groups còn sót lại trong 1 cú click!

Chúc mừng bạn đã hoàn thành xuất sắc siêu dự án triển khai Snaptics API Multi-Stack Architecture trên AWS!
