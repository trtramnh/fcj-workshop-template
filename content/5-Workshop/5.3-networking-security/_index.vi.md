---
title: "Mạng & Bảo mật"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---


Trong kiến trúc Enterprise, tầng Mạng đóng vai trò quyết định. Chúng ta phải bảo vệ luồng truy cập từ bên ngoài và phân phối qua CloudFront, đồng thời tối ưu hóa luồng mạng nội bộ bằng VPC Endpoints để giảm thiểu chi phí băng thông đắt đỏ.

## 1. Quản lý Domain với Route 53 

Nếu bạn đã mua một tên miền thật (ví dụ `snaptics.com`), hãy cấu hình Route 53.
- Mở **Route 53 ➔ Hosted zones ➔ Create hosted zone**.
- Nhập tên miền của bạn. Lấy 4 dòng Name Servers (NS) dán ngược lại vào nơi bạn mua tên miền để trỏ DNS về AWS.
- Sử dụng **AWS Certificate Manager (ACM)** để xin một chứng chỉ SSL miễn phí tại vùng `us-east-1` (Bắt buộc phải là `us-east-1` mới gắn được vào CloudFront).

## 2. CloudFront

Thay vì phơi trần Load Balancer (ALB) ra Internet, ta sẽ giấu nó sau CloudFront để tăng tốc độ phản hồi và ẩn IP thực. *(Lưu ý: Tính năng tường lửa WAF đã được tích hợp sẵn tự động khi chúng ta host Frontend bằng AWS Amplify, nên ta không cần tốn tiền tạo WAF rườm rà ở ngoài nữa).*

### Cấu hình Amazon CloudFront cho API
- Vào **CloudFront ➔ Create Distribution**.
- **Origin domain:** Chọn Application Load Balancer của bạn (Mình sẽ tạo ở phần Compute).
- **Viewer Protocol Policy:** Chọn Redirect HTTP to HTTPS.
- **Cache key and origin requests:** Chọn **Cache policy and origin request policy** ➔ CachingDisabled và AllViewer (bắt buộc với API).
- **Custom SSL Certificate:** Gắn cái chứng chỉ SSL bạn xin được ở bước 1 vào.
- Bấm Create.

## 3. Thiết kế Multi-Tier VPC

Tạo mạng nội bộ (`snaptics-vpc`) với dải IP `10.0.0.0/16`.

Tạo 4 Subnet tại `ap-southeast-1`:
1. `snaptics-public-1a`: `10.0.1.0/24` (Bật auto-assign public IPv4)
2. `snaptics-public-1b`: `10.0.2.0/24` (Bật auto-assign public IPv4)
3. `snaptics-private-1a`: `10.0.3.0/24` (Nơi chứa ECS & Database SQL Server)
4. `snaptics-private-1b`: `10.0.4.0/24` (Nơi chứa ECS & Database SQL Server)

## 4. Gateways & VPC Endpoints 

### A. Internet Gateway & NAT Gateway
- Tạo **Internet Gateway** `snaptics-igw` và gắn vào VPC.
- Tạo **NAT Gateway** `snaptics-nat-gw` nằm ở `snaptics-public-1a`. Cục NAT này có nhiệm vụ giúp Container đứng trong mạng Private có đường Internet để gọi ra API Google Gemini và Azure OCR.
- Sửa Route Tables: Mạng Public trỏ `0.0.0.0/0` ra IGW, mạng Private trỏ `0.0.0.0/0` ra NAT Gateway.

### B. VPC Gateway Endpoint cho S3 (Rất Quan Trọng)
Nếu Container gọi API upload hóa đơn ảnh nặng 5MB lên S3 thông qua vòng lặp NAT Gateway, AWS sẽ tính tiền "Data Processing" rất đắt. Giải pháp là dùng VPC Endpoint!
- Vào **VPC ➔ Endpoints ➔ Create endpoint**.
- Service category: **AWS services**.
- Service: Gõ chữ `s3` và tìm mục có Type là **Gateway** (`com.amazonaws.ap-southeast-1.s3`).
- VPC: `snaptics-vpc`.
- Route tables: Tick chọn bảng định tuyến của mạng **Private**.
- Policy: **Full Access**.
- Bấm Create.
Kể từ giờ, dữ liệu bắn từ Code ECS sang S3 sẽ đi qua đường hầm Endpoint nội bộ, nhanh hơn gấp bội và **Miễn phí 100% băng thông**!

## 5. Security Groups 

Bạn phải cấu hình Firewall cứng (Security Group) theo nguyên tắc từng tầng:

- **ALB Security Group (`snaptics-alb-sg`):** 
  - Mở cổng HTTP (80) và HTTPS (443).
  - *Mẹo bảo mật cao cấp:* Bạn có thể giới hạn Source IP chỉ cho phép dải IP của AWS CloudFront gọi vào, qua đó chặn đứng mọi kẻ lạ cố tình gọi thẳng IP của ALB!
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.3-networking-security/alb-sg.jpg" >
  </div>
- **ECS Security Group (`snaptics-ecs-sg`):**
  - Mở cổng Custom TCP `8080`. Source CHỈ CHO PHÉP gọi từ `snaptics-alb-sg`.
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.3-networking-security/fargate_sg.png" >
  </div>
- **SQL Server Security Group (`snaptics-aurora-sg`):**
  - Mở cổng DB (Ví dụ 1433 nếu dùng SQL Server hoặc 3306/5432). Source CHỈ CHO PHÉP gọi từ `snaptics-ecs-sg`.
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.3-networking-security/db_sg.png" >
  </div>