---
title: "Worklog Tuần 2"
date: 2026-05-25
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2

* Hiểu các thành phần cơ bản của Amazon VPC (IPv4 CIDR, Subnet, Route Table, Internet Gateway).
* Phân biệt Public Subnet và Private Subnet dựa trên cấu hình định tuyến và khả năng kết nối.
* Hiểu vai trò và cơ chế hoạt động của Route Table, Internet Gateway và NAT Gateway.
* Phân biệt Security Group và Network ACL trong bảo mật mạng VPC.
* Xác định định hướng ban đầu, người dùng mục tiêu, phạm vi bài toán và các dịch vụ AWS cho project nhóm.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Trao đổi với 4 thành viên trong nhóm <br> - Thảo luận và đóng góp ý tưởng project AWS <br> - Xác định bài toán thực tế, người dùng mục tiêu và phạm vi triển khai ban đầu <br> - Đánh giá các dịch vụ AWS tiềm năng sẽ sử dụng trong dự án | 25/05/2026 | 25/05/2026 | [Amazon VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) |
| 3 | - Tìm hiểu tổng quan về Amazon VPC và không gian địa chỉ IPv4 CIDR Block <br> - Tìm hiểu khái niệm Public Subnet, Private Subnet, Route Table và Internet Gateway <br> - Phân tích luồng lưu lượng mạng giữa các subnet và Internet <br> - Vẽ sơ đồ kiến trúc mạng VPC cơ bản trước khi triển khai | 26/05/2026 | 26/05/2026 | [Amazon VPC Workshop](https://000003.awsstudygroup.com/) <br> [Amazon VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) <br> [VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html) <br> [Route Tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) <br> [Internet Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) |
| 4 | - Thực hành khởi tạo một Amazon VPC và chia các CIDR block cho Public Subnet và Private Subnet <br> - Tạo và gắn Internet Gateway vào VPC <br> - Cấu hình Route Table cho Public Subnet (định tuyến 0.0.0.0/0 tới Internet Gateway) <br> - Triển khai các EC2 instance vào Public Subnet và Private Subnet để kiểm tra khả năng kết nối mạng | 27/05/2026 | 27/05/2026 | [Amazon VPC Workshop](https://000003.awsstudygroup.com/) <br> [Create EC2 Server](https://000003.awsstudygroup.com/4-createec2server/4.1-createec2/) |
| 5 | - Tìm hiểu và so sánh chi tiết Security Group (instance-level, stateful) và Network ACL (subnet-level, stateless) <br> - Phân tích cấu hình Inbound Rules và Outbound Rules cho từng lớp bảo mật <br> - Thực hành cấu hình Security Group cho EC2 và Network ACL cho Subnet <br> - Kiểm tra lưu lượng mạng (ping, SSH, HTTP) để xác minh cơ chế lọc gói tin | 28/05/2026 | 28/05/2026 | [Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html) <br> [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html) |
| 6 | - Tìm hiểu khái niệm và nguyên lý hoạt động của NAT Gateway <br> - Khởi tạo NAT Gateway trong Public Subnet và cấp phát Elastic IP tĩnh <br> - Cập nhật Route Table của Private Subnet để trỏ lưu lượng 0.0.0.0/0 tới NAT Gateway <br> - Kiểm tra EC2 trong Private Subnet truy cập Internet thành công theo chiều đi ra (outbound) mà không bị truy cập từ bên ngoài (inbound) <br> - Tổng hợp kết quả thực hành và chụp ảnh minh chứng | 29/05/2026 | 29/05/2026 | [NAT Gateway Workshop](https://000003.awsstudygroup.com/4-createec2server/4.3-natgateway/) <br> [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) |

### Kết quả đạt được tuần 2

* Đã trao đổi cùng 4 thành viên trong nhóm, bước đầu xác định ý tưởng dự án, bài toán cần giải quyết, đối tượng người dùng mục tiêu và dự kiến các dịch vụ AWS cốt lõi sẽ áp dụng.
* Hiểu vai trò cơ bản và cấu trúc của Amazon VPC cùng các thành phần như IPv4 CIDR Block, Public Subnet, Private Subnet, Route Table và Internet Gateway.
* Đã hoàn thành sơ đồ kiến trúc mạng VPC và thực hành tạo thành công VPC, chia phân mạng (Public/Private Subnet), gắn Internet Gateway và cấu hình Route Table.
* Làm quen với việc khởi tạo EC2 instance trong các subnet khác nhau để kiểm tra kết nối mạng nội bộ và Internet.
* Hiểu sự khác biệt cơ bản giữa Security Group (stateful, cấp instance) và Network ACL (stateless, cấp subnet), đồng thời thực hành cấu hình luật Inbound/Outbound để kiểm soát lưu lượng.
* Tìm hiểu nguyên lý hoạt động của NAT Gateway, thực hành tạo NAT Gateway trong Public Subnet với Elastic IP và cấu hình Route Table cho Private Subnet, giúp EC2 trong Private Subnet có thể truy cập Internet theo chiều đi ra an toàn.
* Tổng hợp và lưu trữ đầy đủ hình ảnh minh chứng cho từng bước cấu hình VPC và kiểm tra mạng.
