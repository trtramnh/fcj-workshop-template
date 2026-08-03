---
title: "Worklog Tuần 2"
date: 2026-05-18
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2

* Nắm vững các thành phần mạng cốt lõi trong Amazon VPC (IPv4 CIDR Block, Subnet, Route Table, Internet Gateway).
* Phân biệt chi tiết giữa Public Subnet và Private Subnet dựa trên bảng định tuyến và quyền kết nối Internet.
* Tìm hiểu nguyên lý hoạt động và triển khai NAT Gateway cho Private Subnet truy cập Internet chiều đi ra (outbound).
* So sánh cơ chế bảo mật giữa Security Group (Stateful, instance-level) và Network ACL (Stateless, subnet-level).
* Khởi tạo máy chủ Amazon EC2 trong VPC để kiểm tra khả năng kết nối mạng nội bộ và mạng bên ngoài.
* Tìm hiểu tổng quan bài thực hành **Workshop 5** về giải pháp kết nối riêng tư AWS PrivateLink và chuẩn bị môi trường Prerequisite.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tìm hiểu tổng quan dịch vụ Amazon VPC và nguyên lý quy hoạch không gian địa chỉ IP (IPv4 CIDR Block).<br>- Phân tích khái niệm Public Subnet và Private Subnet trong thiết kế hạ tầng mạng đám mây. | 18/05/2026 | 18/05/2026 | [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)<br>[VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html) |
| 3 | - Khởi tạo một Amazon VPC custom với dải CIDR `10.0.0.0/16`.<br>- Tạo Public Subnets và Private Subnets trên 2 Availability Zones khác nhau.<br>- Khởi tạo Internet Gateway (IGW) và thực hiện gắn (attach) vào VPC custom. | 19/05/2026 | 19/05/2026 | [VPC Route Tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)<br>[Internet Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) |
| 4 | - Tạo Route Table cho Public Subnet và thêm đường truyền trỏ `0.0.0.0/0` tới Internet Gateway.<br>- Khởi tạo NAT Gateway trong Public Subnet và gán Elastic IP tĩnh.<br>- Cấu hình Route Table cho Private Subnet định tuyến traffic Internet qua NAT Gateway. | 20/05/2026 | 20/05/2026 | [NAT Gateways Guide](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) |
| 5 | - So sánh chi tiết Security Group (Stateful, lọc theo máy chủ) và Network ACL (Stateless, lọc theo phân mạng).<br>- Cấu hình Inbound/Outbound rules cho Security Group (cho phép HTTP/SSH) và Network ACL.<br>- Khởi tạo các Amazon EC2 instances vào Public Subnet và Private Subnet để kiểm tra. | 21/05/2026 | 21/05/2026 | [Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)<br>[Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html) |
| 6 | - **Thực hành Workshop 5 (Phần 1 & 2):**<br>&emsp; + Tìm hiểu tổng quan bài lab "Đảm bảo truy cập Hybrid an toàn đến S3 bằng VPC Endpoint" ([Workshop Overview](5-Workshop/5.1-Workshop-overview/)).<br>&emsp; + Chuẩn bị môi trường Prerequisite (tạo VPC test, Subnets, EC2 Server và S3 Bucket mẫu) ([Workshop Prerequisite](5-Workshop/5.2-Prerequiste/)).<br>- Kiểm tra kết nối từ EC2 Public sang EC2 Private và chụp ảnh minh chứng thực hành. | 22/05/2026 | 22/05/2026 | [AWS Site-to-Site VPN](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html)<br>[Workshop Overview](5-Workshop/5.1-Workshop-overview/) |

### Kết quả đạt được tuần 2

* Hiểu sâu về cấu trúc Amazon VPC, cách tính toán dải IP CIDR và phân chia Subnet chuẩn kiến trúc AWS.
* Khởi tạo thành công VPC custom tích hợp đầy đủ Public Subnet và Private Subnet trên nhiều Availability Zones.
* Cấu hình bảng định tuyến Route Table kết nối Internet Gateway cho Public Subnet hoạt động ổn định.
* Triển khai thành công NAT Gateway kèm Elastic IP, hỗ trợ các tài nguyên Private truy cập Internet an toàn chiều đi ra.
* Làm chủ kỹ năng cấu hình tường lửa đa lớp kết hợp giữa Security Group và Network ACL.
* Kiểm tra và xác minh thành công khả năng kết nối giữa máy chủ EC2 Public và EC2 Private.
* Nắm vững kiến thức tổng quan bài thực hành Workshop 5 và chuẩn bị thành công hạ tầng Prerequisite cho bài lab VPC Endpoint.
* Lưu trữ bộ hình ảnh sơ đồ kiến trúc và cấu hình mạng VPC phục vụ báo cáo.
