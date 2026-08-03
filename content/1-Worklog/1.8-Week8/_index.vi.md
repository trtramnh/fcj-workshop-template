---
title: "Worklog Tuần 8"
date: 2026-07-06
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8

* Phân tích giải pháp kết nối lai (Hybrid Access từ On-premises đến AWS) sử dụng Interface VPC Endpoints dựa trên công nghệ AWS PrivateLink.
* Thực hành tạo và cấu hình Interface VPC Endpoint cho Amazon S3 theo đúng hướng dẫn Workshop chương 5.4 ("Truy cập đến S3 từ TTDL On-premises").
* Nghiên cứu cơ chế kiểm soát truy cập phân mức bằng VPC Endpoint Policies theo Workshop chương 5.5 ("VPC Endpoint Policies").
* Viết JSON Endpoint Policy giới hạn hành quyền: chỉ cho phép thao tác trên các S3 Bucket chỉ định của tổ chức và chặn truy cập S3 bên ngoài.
* Thực hiện quy trình Dọn dẹp tài nguyên (Cleanup) theo chương 5.6 để tối ưu chi phí hạ tầng và tổng kết toàn bộ báo cáo phần Workshop 5.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Nghiên cứu chuyên sâu công nghệ AWS PrivateLink và Interface VPC Endpoints.<br>- Hiểu cơ chế hoạt động: tạo Elastic Network Interface (ENI) mang địa chỉ IP nội bộ trong Subnet và sử dụng Private DNS.<br>- So sánh sự khác nhau về khả năng định tuyến từ môi trường On-Premises (qua Direct Connect/VPN) tới Gateway Endpoint vs Interface Endpoint. | 06/07/2026 | 06/07/2026 | [AWS PrivateLink Concepts](https://docs.aws.amazon.com/vpc/latest/privatelink/concepts.html)<br>[Interface Endpoints for S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/privatelink-interface-endpoints.html) |
| Thứ 3 | - Thực hành chương 5.4: Tạo Interface VPC Endpoint cho Amazon S3 trên AWS Console.<br>- Cấu hình Security Group cho Interface Endpoint chỉ cho phép cổng HTTPS (443) từ dải mạng On-Premises mô phỏng.<br>- Thực hành cấu hình DNS Endpoint và kiểm tra truy vấn DNS với `nslookup` / `dig` để xác minh địa chỉ IP riêng của ENI. | 07/07/2026 | 07/07/2026 | [Workshop S3 On-Premises](5.4-S3-onprem/)<br>[Private DNS for Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface-endpoints.html) |
| Thứ 4 | - Nghiên cứu chương 5.5: VPC Endpoint Policies - Lớp bảo mật bổ sung cho VPC Endpoint.<br>- Phân tích mối quan hệ kiểm soát giữa IAM Policy, S3 Bucket Policy và VPC Endpoint Policy.<br>- Viết tệp Endpoint Policy bằng JSON giới hạn Principal chỉ cho phép tài khoản công ty truy cập và áp dụng vào Endpoint. | 08/07/2026 | 08/07/2026 | [Workshop Endpoint Policies](5.5-Policy/)<br>[VPC Endpoint Policies](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html) |
| Thứ 5 | - **Thực hành kiểm thử Endpoint Policy:**<br>- Thử nghiệm lệnh `aws s3 ls s3://allowed-company-bucket` -> Thành công.<br>- Thử nghiệm lệnh `aws s3 ls s3://external-personal-bucket` -> Bị từ chối (Access Denied) bởi Endpoint Policy.<br>- Xác nhận khả năng chống rò rỉ dữ liệu (Data Exfiltration Prevention) thông qua VPC Endpoint Policy. | 09/07/2026 | 09/07/2026 | [Workshop Endpoint Policies](5.5-Policy/)<br>[Preventing Data Exfiltration](https://aws.amazon.com/blogs/security/how-to-use-vpc-endpoint-policies-to-prevent-data-exfiltration/) |
| Thứ 6 | - **Dọn dẹp tài nguyên & Hoàn thiện báo cáo:**<br>- Thực hiện dọn dẹp các tài nguyên thực hành theo hướng dẫn chương 5.6 ("Dọn dẹp tài nguyên"): Xóa VPC Endpoints, ENIs, EC2 instances và S3 buckets thử nghiệm.<br>- Kiểm tra lại bảng chi phí AWS Billing đảm bảo không phát sinh chi phí duy trì Interface Endpoint.<br>- Tổng hợp và hoàn thiện mã nguồn, ảnh minh chứng cho toàn bộ Chương 5 Workshop. | 10/07/2026 | 10/07/2026 | [Workshop Cleanup](5.6-Cleanup/)<br>[AWS Cost Allocation](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) |

### Kết quả đạt được tuần 8

* Nắm vững nguyên lý hoạt động của AWS PrivateLink và Interface VPC Endpoint dựa trên ENI và Private DNS resolution.
* Hiểu lý do tại sao Interface VPC Endpoint là giải pháp duy nhất cho phép môi trường On-Premises truy cập dịch vụ AWS S3 qua kết nối riêng tư (Direct Connect/VPN).
* Thực hành tạo thành công Interface VPC Endpoint cho Amazon S3, cấu hình chuẩn xác Security Group cho ENI và kiểm tra truy vấn DNS.
* Làm chủ kỹ thuật viết VPC Endpoint Policies bằng JSON để kiểm soát quyền hạn truy cập tài nguyên S3 ở cấp độ điểm cuối.
* Kiểm chứng thành công tính năng phòng chống thất thoát dữ liệu (Data Exfiltration Prevention): Chặn đứng các yêu cầu truy cập đến S3 bucket nằm ngoài doanh nghiệp.
* Thực hiện dọn dẹp triệt để toàn bộ tài nguyên thử nghiệm theo chương 5.6, đảm bảo tối ưu chi phí và không duy trì tài nguyên thừa.
* Hoàn thành trọn vẹn nội dung Chương 5 Workshop ("Đảm bảo truy cập Hybrid an toàn đến S3 bằng cách sử dụng VPC endpoint") trên giao diện báo cáo Hugo.
