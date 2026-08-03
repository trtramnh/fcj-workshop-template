---
title: "Worklog Tuần 4"
date: 2026-06-08
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4

* Hiểu kiến trúc cơ sở dữ liệu quan hệ quản trị trên Đám mây với dịch vụ Amazon RDS (Relational Database Service).
* Phân biệt các Database Engine hỗ trợ (PostgreSQL, MySQL, MariaDB, Aurora) và tiêu chí lựa chọn cho từng dự án.
* Nắm vững cơ chế Multi-AZ Deployment để đảm bảo tính sẵn sàng cao (High Availability) và Read Replicas giúp mở rộng khả năng đọc dữ liệu.
* Thực hành tạo DB Subnet Group, cấu hình Security Group và triển khai Amazon RDS Instance nằm hoàn toàn trong Private Subnet.
* Kết nối ứng dụng Backend đến Amazon RDS, thực thi script khởi tạo cơ sở dữ liệu và thử nghiệm cơ chế Sao lưu tự động (Automated Backups).

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Tìm hiểu tổng quan về dịch vụ cơ sở dữ liệu quan hệ Amazon RDS.<br>- So sánh việc tự quản trị DB trên EC2 và việc sử dụng Amazon RDS tĩnh tự động hóa.<br>- Tìm hiểu các khái niệm: DB Instance Class, Storage Types (gp2, gp3, io1), DB Parameter Group và Option Group. | 08/06/2026 | 08/06/2026 | [Amazon RDS User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html)<br>[RDS Storage Types](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Storage.html) |
| Thứ 3 | - Nghiên cứu tính năng Multi-AZ Deployments (Synchronous Replication) giúp khắc phục sự cố tự động (Failover).<br>- Tìm hiểu Read Replicas (Asynchronous Replication) hỗ trợ giảm tải truy vấn đọc cho Master Database.<br>- Phân tích mô hình kiến trúc hạ tầng DB chuẩn doanh nghiệp trên AWS. | 09/06/2026 | 09/06/2026 | [RDS High Availability (Multi-AZ)](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)<br>[RDS Read Replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html) |
| Thứ 4 | - Thực hành tạo DB Subnet Group gồm các Private Subnet thuộc các Availability Zone khác nhau.<br>- Cấu hình Security Group cho RDS chỉ cho phép lưu lượng truy cập từ Security Group của EC2 Web Server (Inbound Port 5432/3306).<br>- Khởi tạo Amazon RDS PostgreSQL instance trong Private Subnet, vô hiệu hóa Public Accessibility. | 10/06/2026 | 10/06/2026 | [RDS Subnet Groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html)<br>[RDS Security Groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.RDSSecurityGroups.html) |
| Thứ 5 | - Thực hành kết nối đến RDS PostgreSQL từ EC2 Server trong Public Subnet thông qua `psql` / pgAdmin.<br>- Triển khai các câu lệnh SQL Migration khởi tạo bảng, index và dữ liệu mẫu cho ứng dụng.<br>- Cấu hình biến môi trường kết nối Database Connection String an toàn trên máy chủ backend. | 11/06/2026 | 11/06/2026 | [Connect to RDS Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ConnectToInstance.html)<br>[PostgreSQL on RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html) |
| Thứ 6 | - **Thực hành & Kiểm thử:**<br>- Tìm hiểu cơ chế Automated Backups (RPO/RTO) và DB Snapshots thủ công.<br>- Thử nghiệm tạo Snapshot thủ công và khôi phục (Restore) thử một DB Instance mới từ Snapshot.<br>- Kiểm tra lại toàn bộ Security Group rules đảm bảo không có cổng DB nào mở công khai ra Internet.<br>- Chụp ảnh minh chứng kết quả triển khai RDS và lưu trữ tài liệu. | 12/06/2026 | 12/06/2026 | [RDS Backups & Snapshots](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html)<br>[Restoring DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_RestoreFromSnapshot.html) |

### Kết quả đạt được tuần 4

* Hiểu rõ ưu điểm vượt trội của Amazon RDS so với việc tự cài đặt và quản trị cơ sở dữ liệu trên máy chủ EC2 truyền thống.
* Nắm vững nguyên lý hoạt động của Multi-AZ Deployments và Read Replicas trong việc xây dựng hệ thống cơ sở dữ liệu có khả năng chống lỗi cao và mở rộng linh hoạt.
* Thiết lập thành công DB Subnet Group bao phủ nhiều Availability Zones để chuẩn bị cho kiến trúc Multi-AZ.
* Triển khai thành công Amazon RDS PostgreSQL Instance đặt hoàn toàn trong Private Subnet, khóa hoàn toàn Public Access.
* Cấu hình chính xác Security Group rule theo mô hình phân lớp (Chỉ cho phép EC2 Security Group kết nối tới DB Port 5432).
* Thực hiện kết nối thành công từ ứng dụng Backend trên EC2 tới RDS Instance và hoàn tất chạy script SQL Migration khởi tạo dữ liệu.
* Thao tác thành công việc tạo DB Snapshot thủ công và hiểu rõ quy trình restore khôi phục dữ liệu khi gặp sự cố.
* Lưu trữ đầy đủ bộ ảnh minh chứng kiến trúc và cấu hình chi tiết của Amazon RDS.
