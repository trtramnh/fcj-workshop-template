---
title: "Worklog Tuần 3"
date: 2026-05-25
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3

* Tìm hiểu chuyên sâu về dịch vụ tính toán Amazon EC2 (Instance Types, AMI, EBS Volume types).
* Thực hành kết nối máy chủ EC2 qua giao thức SSH và chạy thử ứng dụng web mẫu (Nginx).
* Nghiên cứu dịch vụ cơ sở dữ liệu quản trị Amazon RDS (PostgreSQL/SQL Server, Multi-AZ Deployment, Read Replicas).
* Triển khai Amazon RDS Instance nằm hoàn toàn trong Private Subnet của VPC.
* Thử nghiệm kết nối an toàn từ máy chủ EC2 đến cơ sở dữ liệu Amazon RDS qua Security Group.
* Thực hành bài lab **Workshop 5.3**: Tạo Gateway VPC Endpoint kết nối máy chủ EC2 với Amazon S3 trong môi trường nội bộ VPC không đi qua Internet.
* Thực hành quy trình Sao lưu (DB Snapshot) và Khôi phục (Restore) cơ sở dữ liệu trên Amazon RDS.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu chuyên sâu về Amazon EC2: phân loại Instance Types (t3.micro, c5, r5), Amazon Machine Image (AMI).<br>- Tìm hiểu các loại ổ đĩa Amazon EBS (gp3, io2, st1) và các đặc tính IOPS, Throughput. | 25/05/2026 | 25/05/2026 | [Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)<br>[Amazon EBS Volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html) |
| 3 | - Khởi tạo máy chủ Amazon EC2 (Ubuntu/Amazon Linux 2023) và gán Key Pair bảo mật.<br>- Kết nối máy chủ qua SSH từ máy cá nhân.<br>- Cài đặt dịch vụ Nginx/Web server mẫu và kiểm tra truy cập HTTP qua địa chỉ Public IP. | 26/05/2026 | 26/05/2026 | [Connect to Linux Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) |
| 4 | - Nghiên cứu dịch vụ cơ sở dữ liệu quản trị Amazon RDS (Relational Database Service).<br>- Phân biệt việc tự quản trị DB trên EC2 và việc dùng dịch vụ Managed RDS.<br>- Tìm hiểu cơ chế sao chép Multi-AZ (High Availability) và Read Replicas (mở rộng khả năng đọc). | 27/05/2026 | 27/05/2026 | [Amazon RDS User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html)<br>[RDS Multi-AZ Deployments](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) |
| 5 | - **Thực hành Workshop 5 (Phần 3 - Truy cập S3 từ VPC):**<br>&emsp; + Khởi tạo Gateway VPC Endpoint cho dịch vụ Amazon S3 ([Workshop S3 from VPC](5-Workshop/5.3-S3-vpc/)).<br>&emsp; + Cập nhật Route Table của Private Subnet, định tuyến traffic `com.amazonaws.<region>.s3` về Gateway Endpoint.<br>&emsp; + Chạy lệnh AWS CLI (`aws s3 ls`) từ máy chủ EC2 Private xác nhận kết nối nội bộ thành công đến S3 mà không đi qua Public Internet. | 28/05/2026 | 28/05/2026 | [Workshop S3 from VPC](5-Workshop/5.3-S3-vpc/)<br>[VPC Gateway Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-s3.html) |
| 6 | - Khởi tạo DB Subnet Group và triển khai Amazon RDS Instance vào Private Subnet.<br>- Thực hành kết nối DB Client từ EC2 Server tới RDS Instance trong Private Subnet.<br>- Thực hiện tạo bản sao lưu thủ công (DB Snapshot) trên Amazon RDS Console và khôi phục (Restore) thử nghiệm.<br>- Tổng kết kiến thức hạ tầng AWS chuẩn bị cho giai đoạn dự án. | 29/05/2026 | 29/05/2026 | [RDS Backups & Restore](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html) |

### Kết quả đạt được tuần 3

* Nắm rõ thông số kỹ thuật cốt lõi của Amazon EC2 và tiêu chí chọn lựa Instance Type cũng như EBS Volume phù hợp.
* Khởi tạo, vận hành và cấu hình web server Nginx mẫu thành công trên máy chủ EC2.
* Hiểu sâu về lợi ích của dịch vụ Amazon RDS trong việc tự động hóa backup, patching và đảm bảo tính sẵn sàng cao.
* Thực hành thành công bài lab Workshop 5.3: Tạo Gateway VPC Endpoint cho S3 và kiểm tra kết nối riêng tư thành công từ EC2 Private tới S3 Bucket.
* Khởi tạo thành công Amazon RDS Instance đặt an toàn trong Private Subnet của VPC custom.
* Cấu hình chính xác quy tắc bảo mật nối chuỗi (Security Group chaining) giữa EC2 và RDS.
* Thành thạo quy trình tạo DB Snapshot và khôi phục cơ sở dữ liệu khi gặp sự cố.
* Hệ thống hóa toàn bộ kiến thức hạ tầng AWS cốt lõi tích lũy trong 3 tuần đầu tiên.
