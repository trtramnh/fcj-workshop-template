---
title: "Gateways & Định tuyến"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---


Private Subnet bản chất là mạng kín, các container bên trong (như ECS Snaptics) sẽ không thể chủ động gọi ra ngoài Google Gemini API hay tải thư viện về nếu không có lối thoát (Outbound traffic). Giải pháp là dùng NAT Gateway.

### 1. Internet Gateway (IGW)
IGW là cánh cửa kết nối toàn bộ VPC với Internet.
- Vào **VPC ➔ Internet Gateways ➔ Create internet gateway**.
- Đặt tên: `snaptics-igw`.
- Chọn IGW vừa tạo ➔ **Actions ➔ Attach to VPC** ➔ Chọn `snaptics-vpc`.

### 2. NAT Gateway
NAT Gateway đóng vai trò làm trung gian, nhận luồng request từ Private Subnet, thay mặt nó gọi ra ngoài qua IGW, rồi trả kết quả về.
- Vào **VPC ➔ NAT Gateways ➔ Create NAT gateway**.
- **Name:** `snaptics-nat-gw`
- **Subnet:** Chọn `snaptics-public-1a` (Bắt buộc phải đặt NAT ở mạng Public).
- **Elastic IP allocation ID:** Bấm **Allocate Elastic IP** để mua một IP tĩnh của AWS gắn cho NAT.

### 3. Route Tables (Bảng Định Tuyến)
Chúng ta cần chỉ đường cho các Subnet biết phải gửi traffic đi đâu.

**Tạo Route Table cho Public Subnets:**
- Tạo Route Table tên `snaptics-public-rt`.
- Chọn **Routes ➔ Edit routes**: Trỏ Destination `0.0.0.0/0` ra Target là `snaptics-igw`.
- Chọn **Subnet associations**: Gắn vào 2 mạng `snaptics-public-1a` và `snaptics-public-1b`.

**Tạo Route Table cho Private Subnets:**
- Tạo Route Table tên `snaptics-private-rt`.
- Chọn **Routes ➔ Edit routes**: Trỏ Destination `0.0.0.0/0` ra Target là `snaptics-nat-gw`.
- Chọn **Subnet associations**: Gắn vào 2 mạng `snaptics-private-1a` và `snaptics-private-1b`.
