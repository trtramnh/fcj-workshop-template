---
title: "Amazon RDS SQL Server"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---


<!-- TODO: Chèn ảnh chụp giao diện Amazon RDS (chỗ có link Endpoint) vào đây -->
![RDS Database](/images/5-Workshop/placeholder-rds.png)

> [!NOTE]
> **Khác biệt so với Sơ đồ Kiến trúc (Architecture Diagram):** Trong sơ đồ tổng thể, hệ thống RDS được vẽ theo chuẩn Production với cơ chế **Multi-AZ (Primary & Standby)** để dự phòng lỗi. Tuy nhiên, trong phạm vi bài Workshop thực hành này, chúng ta sẽ chỉ tạo bản **Single-AZ (Free Tier)** nhằm mục đích tối ưu hóa và tiết kiệm chi phí demo.

Dự án Snaptics sử dụng C# với Entity Framework Core, do đó Microsoft SQL Server là lựa chọn tối ưu nhất.

### 1. Tạo DB Subnet Group
RDS yêu cầu bạn phải nhóm các Subnet lại để nó biết được có thể đặt Database ở đâu.
- Mở **Amazon RDS ➔ Subnet groups ➔ Create DB subnet group**.
- **Name:** `snaptics-db-subnet-group`
- **VPC:** Chọn `snaptics-vpc`
- **Subnets:** Ở phần Add subnets, chọn 2 Availability Zones `ap-southeast-1a` và `ap-southeast-1b`, sau đó tick chọn 2 **Private Subnet** (CIDR `10.0.3.0/24` và `10.0.4.0/24`).

### 2. Tạo RDS Database
- Mở **RDS ➔ Databases ➔ Create database**.
- **Engine options:** Chọn **Microsoft SQL Server** (Bản SQL Server Express Edition nếu bạn muốn dùng Free Tier).
- **Templates:** Free tier (Hoặc Production nếu có kinh phí).
- **Settings:**
  - DB instance identifier: `snaptics-db`
  - Master username: `admin`
  - Master password: `Snaptics@StrongPass123!` (Ghi nhớ pass này).
- **Instance configuration:** `db.t3.micro`.
- **Storage:** 20 GB General Purpose SSD (gp2).
- **Connectivity:**
  - VPC: Chọn `snaptics-vpc`.
  - Subnet group: Chọn `snaptics-db-subnet-group`.
  - **Public access:** Chọn **No** (Bắt buộc).
  - VPC security group: Chọn **Choose existing** và chọn `snaptics-rds-sg`.
- Kéo xuống dưới cùng và click **Create database**.

Quá trình khởi tạo SQL Server trên AWS mất khoảng 15-20 phút. Sau khi trạng thái chuyển sang **Available**, click vào database và copy chuỗi **Endpoint** (Ví dụ: `snaptics-db.cx1y2z3.ap-southeast-1.rds.amazonaws.com`).
