---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


#### Tổng quan

**Snaptics** là một hệ thống lõi tài chính thông minh, được xây dựng trên nền tảng **.NET 8/9**. Hệ thống tích hợp các dịch vụ AI hàng đầu (Google Gemini, Azure Document Intelligence) để trích xuất dữ liệu hóa đơn, kết hợp kiến trúc hướng sự kiện (Event-Driven) với AWS SQS/SNS và xử lý tác vụ nền bằng Hangfire.

Trong workshop nâng cao này, chúng ta sẽ áp dụng mô hình **Multi-Stack Architecture** để triển khai toàn bộ hệ thống Snaptics lên AWS. Kiến trúc được bóc tách thành nhiều tầng rõ rệt: Network, Security, Database, Storage, Compute và Deployment. Việc triển khai Backend API sẽ được thực hiện dưới dạng Serverless Containers trên **Amazon ECS Fargate**, giúp loại bỏ hoàn toàn gánh nặng quản lý hạ tầng máy chủ ảo (EC2).

#### Mô hình Cấu trúc Workshop

Bài workshop được tổ chức thành các phân hệ lõi như sau:

1. **Tổng quan & Kiến trúc (Overview & Architecture)**: Phân tích thiết kế hệ thống và dự toán chi phí.
2. **Chuẩn bị (Prerequisites)**: Thiết lập môi trường Local và phân quyền IAM.
3. **Mạng & Bảo mật (VPC & Security)**: Xây dựng nền tảng mạng an toàn với Public/Private Subnets.
4. **Database, Storage & Secrets**: Triển khai Amazon RDS SQL Server, S3 và SSM Parameter Store.
5. **AI & Tác vụ nền (Messaging)**: Tích hợp AI Insights và cấu hình hàng đợi SQS/SNS.
6. **Compute & Load Balancing (ECS)**: Container hóa ứng dụng .NET và chạy trên Fargate.
7. **Tự động hóa (CI/CD)**: Cơ chế Blue/Green Deployment với script tự động hóa.
8. **Kiểm thử (E2E Testing)**: Xác thực luồng dữ liệu thời gian thực qua SignalR và Swagger.
9. **Dọn dẹp (Cleanup)**: Thu hồi tài nguyên AWS.