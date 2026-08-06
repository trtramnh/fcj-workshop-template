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

**1. Tạo Internet Gateway (`snaptics-igw`):**
- Vào **VPC ➔ Internet Gateways ➔ Create internet gateway**.
- Name: `snaptics-igw`.
- Bấm **Create internet gateway**.
- Chọn IGW vừa tạo → **Actions ➔ Attach to VPC** → chọn `snaptics-vpc` → **Attach internet gateway**.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.3-networking-security/vpc_internetgateway_create_internetgateway.jpg" >
  </div>

**2. Tạo NAT Gateway (`snaptics-nat-gw`):**
- Vào **VPC ➔ NAT Gateways ➔ Create NAT gateway**.
- Name: `snaptics-nat-gw`.
- Subnet: chọn **Public subnet** `snaptics-public-1a`.
- Connectivity type: **Public**.
- VPC tự động là `snaptics-vpc` (dựa theo subnet đã chọn).
- Elastic IP: bấm **Allocate Elastic IP** để tạo IP tĩnh cho NAT Gateway.
- Cuộn xuống bấm **Create NAT gateway** và chờ status từ **Pending** chuyển sang **Available**.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.3-networking-security/vpc_natgateway.jpg" >
  </div>

**3. Cấu hình Route Tables:**
- Mạng Public trỏ `0.0.0.0/0` ra Internet Gateway.
- Mạng Private trỏ `0.0.0.0/0` ra NAT Gateway.

NAT Gateway có nhiệm vụ giúp các Container ECS trong mạng Private có đường Internet để gọi ra API Google Gemini và Azure OCR.

### B. VPC Gateway Endpoint cho S3 (Rất Quan Trọng)
Nếu Container gọi API upload hóa đơn ảnh nặng 5MB lên S3 thông qua vòng lặp NAT Gateway, AWS sẽ tính tiền "Data Processing" rất đắt. Giải pháp là dùng VPC Endpoint để dữ liệu đi qua đường hầm nội bộ của AWS (nhanh hơn và **Miễn phí 100% băng thông**).

**Các bước thực hiện Lab:**
- Vào **VPC ➔ Endpoints ➔ Create endpoint**.
- **Name tag:** `snaptics-s3-endpoint`.
- **Type:** AWS service.
- **Service:** Gõ chữ `s3` vào ô tìm kiếm, sau đó tích chọn `com.amazonaws.ap-southeast-1.s3` có type là **Gateway**.
- **VPC:** chọn `snaptics-vpc`.
- **Route tables:** Tick chọn bảng định tuyến của mạng **Private**.
- **Policy:** **Full Access**.
- Cuộn xuống bấm **Create endpoint**.


  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.3-networking-security/vpc_endpoint_s3.jpg" >
  </div>

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