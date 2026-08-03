---
title: "Worklog Tuần 5"
date: 2026-06-15
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5

* Hiểu giải pháp giám sát và ghi log tập trung trên AWS thông qua dịch vụ Amazon CloudWatch (Metrics, Logs, Alarms, Dashboards).
* Cấu hình CloudWatch Alarms và kết hợp với Amazon Simple Notification Service (SNS) để gửi cảnh báo tự động khi tài nguyên quá tải.
* Tìm hiểu dịch vụ AWS CloudTrail phục vụ kiểm toán hệ thống, theo dõi lịch sử gọi API và quản trị an ninh thông tin.
* Làm chủ các công cụ quản lý chi phí AWS Cost Management: AWS Cost Explorer, AWS Budgets và quy trình thiết lập cảnh báo vượt ngân sách.
* Thực hành cài đặt CloudWatch Agent trên máy chủ EC2, đẩy Application Log lên CloudWatch Logs và thiết kế Dashboard giám sát hạ tầng.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Tìm hiểu tổng quan về Amazon CloudWatch: Metrics, Log Groups, Log Streams, Alarms và Dashboards.<br>- Phân biệt Basic Monitoring (5 phút) và Detailed Monitoring (1 phút) trên EC2.<br>- Phân tích các chỉ số tài nguyên quan trọng: CPUUtilization, StatusCheckFailed, NetworkIn/NetworkOut. | 15/06/2026 | 15/06/2026 | [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)<br>[CloudWatch Concepts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html) |
| Thứ 3 | - Tìm hiểu dịch vụ Amazon SNS (Simple Notification Service) và mô hình Pub/Sub.<br>- Tạo SNS Topic `CloudWatch-Alerts-Topic` và đăng ký Subscription nhận thông báo qua Email.<br>- Thực hành tạo CloudWatch Alarm phát hiện khi CPU EC2 vượt quá 80% trong 5 phút và kích hoạt SNS gửi Email cảnh báo. | 16/06/2026 | 16/06/2026 | [Amazon SNS User Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)<br>[CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) |
| Thứ 4 | - Nghiên cứu dịch vụ AWS CloudTrail phục vụ ghi nhận nhật ký hoạt động (Audit Logs).<br>- Phân tích cấu trúc của một CloudTrail Event (User Identity, Event Time, Event Source, Source IP, Request Parameters).<br>- Tạo một CloudTrail Trail mới đẩy toàn bộ lịch sử thao tác API vào Amazon S3 Bucket để lưu trữ an toàn. | 17/06/2026 | 17/06/2026 | [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)<br>[CloudTrail Events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html) |
| Thứ 5 | - Nghiên cứu nhóm công cụ quản lý chi phí AWS Cost Management.<br>- Sử dụng AWS Cost Explorer để phân tích chi phí tiêu thụ theo từng dịch vụ AWS và từng Region.<br>- Cấu hình AWS Budgets để thiết lập ngưỡng ngân sách hàng tháng (Monthly Budget Limit) và cài đặt cảnh báo khi chi phí vượt 80% dự kiến. | 18/06/2026 | 18/06/2026 | [AWS Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-whatis.html)<br>[AWS Budgets Guide](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |
| Thứ 6 | - **Thực hành & Giám sát hệ thống:**<br>- Thực hành cài đặt CloudWatch Unified Agent trên máy chủ EC2 Linux.<br>- Cấu hình Agent đẩy các chỉ số RAM/Disk utilization và System Logs (`/var/log/syslog`) lên CloudWatch Logs.<br>- Thiết kế CloudWatch Dashboard tập trung hiển thị biểu đồ CPU, RAM, Disk và RDS Connections.<br>- Lưu trữ tài liệu minh chứng quá trình cấu hình. | 19/06/2026 | 19/06/2026 | [CloudWatch Agent Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html)<br>[CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) |

### Kết quả đạt được tuần 5

* Nắm vững cấu trúc và nguyên lý hoạt động của hệ thống giám sát tập trung Amazon CloudWatch.
* Tạo thành công Amazon SNS Topic và cấu hình Email Subscription để tiếp nhận cảnh báo hệ thống tức thời.
* Cấu hình hoàn chỉnh CloudWatch Alarm theo dõi CPU Utilization và kích hoạt thông báo tự động qua Email khi tài nguyên chạm ngưỡng quá tải.
* Hiểu vai trò của AWS CloudTrail trong việc tuân thủ bảo mật, kiểm toán hành vi người dùng và lưu trữ audit log an toàn trên S3.
* Làm chủ giao diện AWS Cost Explorer, nắm rõ phân bổ chi phí hạ tầng và thiết lập thành công AWS Budget Alert ngăn chặn rủi ro vượt ngân sách.
* Cài đặt thành công CloudWatch Agent trên EC2, thu thập thêm các chỉ số nâng cao (Memory & Disk usage) và đẩy log hệ thống về CloudWatch Logs.
* Xây dựng giao diện CloudWatch Dashboard trực quan, giúp theo dõi toàn diện sức khỏe hệ thống (EC2, RDS, Network) trên một màn hình duy nhất.
