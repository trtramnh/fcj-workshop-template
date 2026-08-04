---
title: "Thiết kế VPC & Subnets"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---


<!-- TODO: Chèn ảnh chụp màn hình giao diện AWS VPC Dashboard (danh sách Subnet) của bạn vào đây -->
![VPC Dashboard](/images/5-Workshop/placeholder-vpc.png)
Đầu tiên, chúng ta tạo một Virtual Private Cloud đóng vai trò là "biên giới mạng" cho toàn bộ hệ thống Snaptics.

Vào **AWS Console ➔ VPC ➔ Your VPCs ➔ Create VPC**:
- **Name tag:** `snaptics-vpc`
- **IPv4 CIDR block:** `10.0.0.0/16` (Cấp phát 65,536 địa chỉ IP).

### Quy hoạch Subnet
Hệ thống triển khai Multi-AZ (Đa vùng sẵn sàng) tại Region `ap-southeast-1` (Singapore) nên chúng ta cần tạo 4 Subnet:

Vào **VPC ➔ Subnets ➔ Create subnet** và tạo lần lượt:

| Tên Subnet (Name) | Availability Zone | IPv4 CIDR block | Chức năng chính |
| :--- | :--- | :--- | :--- |
| **`snaptics-public-1a`** | `ap-southeast-1a` | `10.0.1.0/24` | Chứa Application Load Balancer và NAT Gateway. |
| **`snaptics-public-1b`** | `ap-southeast-1b` | `10.0.2.0/24` | Chứa Application Load Balancer (Cho tính khả dụng cao). |
| **`snaptics-private-1a`** | `ap-southeast-1a` | `10.0.3.0/24` | Chứa ECS Fargate Task 1 và RDS Primary. |
| **`snaptics-private-1b`** | `ap-southeast-1b` | `10.0.4.0/24` | Chứa ECS Fargate Task 2 và RDS Replica. |

> [!IMPORTANT]
> Đối với 2 mạng `snaptics-public-1a` và `snaptics-public-1b`, sau khi tạo xong, hãy tick chọn Subnet ➔ Actions ➔ **Edit subnet settings** ➔ Bật **Enable auto-assign public IPv4 address**.
