---
title: "Worklog Tuần 1"
date: 2026-05-18
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Làm quen với chương trình First Cloud Journey và các thành viên trong nhóm.
* Đọc và nắm rõ các nội quy, quy định của đơn vị thực tập.
* Tìm hiểu tổng quan về Cloud Computing và các nhóm dịch vụ cốt lõi của AWS (Compute, Storage, Networking, Database).
* Hướng dẫn tạo và bảo mật tài khoản AWS Free Tier, cài đặt AWS CLI và tìm hiểu các thành phần cơ bản của Amazon EC2.
* Tìm nhóm để làm project liên quan tới AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Làm quen với các thành viên FCAJ <br> - Đọc và ghi nhớ nội quy, quy định tại đơn vị thực tập | 18/05/2026 | 18/05/2026 | [Nội quy FCAJ](https://hcm-rules.awsfcaj.com/1-regulations/) |
| 3 | - Tìm hiểu tổng quan về Điện toán đám mây (Cloud Computing) và AWS <br> - Tìm hiểu các nhóm dịch vụ AWS chính: <br>&emsp; + Compute <br>&emsp; + Storage <br>&emsp; + Networking <br>&emsp; + Database | 19/05/2026 | 19/05/2026 | [AWS Cloud Journey](https://cloudjourney.awsstudygroup.com/) <br> [Tổng quan AWS](https://aws.amazon.com/what-is-aws/) |
| 4 | - Tạo và bảo mật tài khoản AWS Free Tier (bật MFA & kiểm tra thông tin thanh toán) <br> - Làm quen với giao diện AWS Management Console <br> - Cài đặt và cấu hình default profile cho AWS CLI | 20/05/2026 | 20/05/2026 | [AWS Cloud Journey](https://cloudjourney.awsstudygroup.com/) <br> [AWS Free Tier](https://aws.amazon.com/free/) <br> [Tài liệu AWS CLI](https://docs.aws.amazon.com/cli/) |
| 5 | - Tìm hiểu kiến thức cơ bản về Amazon EC2: <br>&emsp; + AMI <br>&emsp; + Instance Types <br>&emsp; + EBS <br>&emsp; + Security Group <br>&emsp; + Key Pair <br>&emsp; + Elastic IP | 21/05/2026 | 21/05/2026 | [AWS Cloud Journey](https://cloudjourney.awsstudygroup.com/) <br> [Tài liệu Amazon EC2](https://docs.aws.amazon.com/ec2/) |
| 6 | - **Thực hành:** <br>&emsp; + Khởi tạo một Amazon EC2 instance <br>&emsp; + Kết nối đến EC2 qua SSH <br>&emsp; + Tạo, gắn và kiểm tra EBS Volume | 22/05/2026 | 22/05/2026 | [AWS Cloud Journey](https://cloudjourney.awsstudygroup.com/) <br> [Tài liệu Amazon EBS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html) |

### Kết quả đạt được tuần 1:

* Hiểu tổng quan về Điện toán đám mây và các nhóm dịch vụ AWS cơ bản:
  * Compute
  * Storage
  * Networking
  * Database

* Đã tạo thành công và bảo mật tài khoản AWS Free Tier, thiết lập bảo mật Multi-Factor Authentication (MFA) và kiểm tra thông tin thanh toán.

* Trở nên quen thuộc với việc sử dụng AWS Management Console để tìm kiếm và điều hướng các dịch vụ.

* Cài đặt thành công AWS CLI trên máy tính cá nhân và cấu hình default profile (bao gồm `Access Key`, `Secret Key`, `Default Region` và `Output Format`).

* Kiểm tra và xác nhận cấu hình AWS CLI bằng các lệnh cơ bản:
  ```bash
  aws --version
  aws configure
  aws configure list
  aws sts get-caller-identity
  aws ec2 describe-regions
  ```

* Học được cách khởi tạo một Amazon EC2 instance từ giao diện console.

* Kết nối thành công tới EC2 instance bằng giao thức SSH thông qua Key Pair.

* Thực hành thành công việc tạo mới, gắn (attach) và kiểm tra EBS Volume trên EC2 instance.

* Nắm được vai trò cơ bản của các thành phần AMI, Security Group, Key Pair và Elastic IP trong quản lý tài nguyên EC2.

* Tìm được nhóm gồm 4 thành viên và chuẩn bị sẳn sàng để làm project.