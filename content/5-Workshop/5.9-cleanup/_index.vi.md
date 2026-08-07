---
title: "Dọn dẹp Tài nguyên"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---


Vì đây là kiến trúc Enterprise siêu to khổng lồ, việc để nó chạy qua đêm sẽ "đốt" của bạn một lượng tiền không nhỏ (đặc biệt là Database SQL Server Multi-AZ, WAF và NAT Gateway). BẠN BẮT BUỘC PHẢI DỌN DẸP NGAY SAU KHI THỰC HÀNH XONG!

Hãy làm đúng trình tự ngược từ ngoài vào trong dưới đây để tránh bị lỗi "tài nguyên đang bị khóa":

### 1. Tầng Frontend & DNS
- **AWS Amplify:** Vào giao diện Amplify, chọn App Snaptics, bấm Actions ➔ **Delete app**.
- **AWS WAF:** Vào mục Web ACLs, chọn `snaptics-waf-acl` và bấm **Delete**.
- **Route 53:** Xóa các Record CNAME/A bạn đã trỏ. (Đừng xóa Hosted Zone nếu đó là domain bạn mua bằng tiền).

### 2. Tầng Compute & Load Balancer
- **ECS Fargate:** Vào `Snaptics-Cluster`, sửa `Desired tasks` của Service về `0`. Đợi cho các máy ảo tắt ngấm. Xóa Service. Sau đó xóa trắng Cluster.
- **Load Balancer:** Vào EC2 ➔ Load Balancers, xóa `snaptics-alb`.
- **Target Groups:** Nhảy qua tab Target Groups, xóa `snaptics-ecs-tg`.

### 3. Tầng Dữ liệu & Storage (Nơi đốt tiền nhiều nhất)
- **Amazon RDS for SQL Server:** Vào RDS ➔ Databases. Chọn cụm `snaptics-sql-server`. Bấm Actions ➔ **Delete**. *Cực kỳ cẩn thận: BỎ tick dòng "Create final snapshot", tick đồng ý mọi cảnh báo rủi ro, gõ chữ `delete me` vào ô xác nhận.*
- **AWS Systems Manager Parameter Store:** Chọn `/snaptics/prod/db-connection` ➔ Actions ➔ **Schedule secret deletion** (Hẹn 7 ngày sau tự xóa vĩnh viễn).
- **Amazon S3:** Vào Bucket của bạn. Bấm **Empty** để dọn sạch rác hóa đơn ảnh bên trong. Sau đó mới bấm **Delete** Bucket được.
- **Amazon ECR:** Xóa Repository chứa file Docker.
- **SQS & SNS:** Xóa hàng đợi `snaptics-ai-queue` (và cái DLQ của nó), xóa luôn Topic cảnh báo.

### 4. Tầng Mạng nội bộ (VPC)
- **NAT Gateway:** Vào VPC ➔ NAT Gateways. Bấm xóa `snaptics-nat-gw`.
- **Elastic IP (Rất dễ quên):** Chờ 3 phút cho cục NAT bay màu hẳn. Tiếp tục vào mục **Elastic IPs**, check chọn cái IP tĩnh đó và bấm **Release IP**. (Quên bước này AWS trừ 1 đô/tháng tiền thuê IP tĩnh).
- **VPC Endpoints:** Vào mục Endpoints, xóa cái S3 Gateway Endpoint.
- **VPC:** Cuối cùng, vào VPC ➔ Your VPCs. Chọn `snaptics-vpc` và bấm **Delete VPC**. Cú click quyền lực này sẽ tự động thu gom và đốt sạch toàn bộ Subnets, Route Tables, IGW và Security Groups còn sót lại!

### 5. Khóa bảo mật (IAM & GitHub)
- **IAM:** Xóa user `github-actions-snaptics` và 2 cái Role của ECS để đóng cửa hoàn toàn quyền truy cập.
- **GitHub Secrets:** Vào repo GitHub xóa 2 cái khóa AWS đi cho an toàn.

Chúc mừng bạn đã hoàn thành xuất sắc khóa huấn luyện triển khai (và tiêu hủy) một hệ thống AWS Enterprise thực thụ!