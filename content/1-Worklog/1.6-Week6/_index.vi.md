---
title: "Worklog Tuần 6"
date: 2026-06-22
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6

* Hiểu khái niệm Hạ tầng dưới dạng Mã nguồn (Infrastructure as Code - IaC) và lợi ích của dịch vụ AWS CloudFormation.
* Nắm vững cấu trúc khai báo mẫu CloudFormation Template (YAML/JSON) bao gồm các phần: Parameters, Resources, Outputs và Mappings.
* Làm chủ kịch bản tự động hóa tài nguyên bằng AWS CLI kết hợp với bộ lọc truy vấn `--query` và định dạng dữ liệu JSON.
* Nghiên cứu cơ chế Quản lý vòng đời CloudFormation Stack, Cập nhật Stack (Stack Updates) và Phát hiện sai lệch cấu hình (Drift Detection).
* Thực hành triển khai tự động toàn bộ hạ tầng 2-tier (VPC, Subnets, Security Groups, EC2, RDS) chỉ bằng một CloudFormation Template thông qua AWS CLI.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Tìm hiểu khái niệm Hạ tầng dưới dạng Mã nguồn (IaC) và tổng quan AWS CloudFormation.<br>- So sánh việc khởi tạo thủ công qua Console với việc tự động hóa bằng CloudFormation Template.<br>- Nghiên cứu các thành phần chính trong tệp YAML: AWSTemplateFormatVersion, Description, Parameters, Resources, Outputs. | 22/06/2026 | 22/06/2026 | [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)<br>[Template Anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html) |
| Thứ 3 | - Thực hành viết CloudFormation Template YAML khai báo hạ tầng mạng VPC cơ bản.<br>- Khai báo các tài nguyên: AWS::EC2::VPC, AWS::EC2::Subnet, AWS::EC2::InternetGateway, AWS::EC2::RouteTable và AWS::EC2::SubnetRouteTableAssociation.<br>- Sử dụng hàm nội hàm (Intrinsic Functions) như `!Ref`, `!Sub`, `!GetAtt` để liên kết phụ thuộc giữa các tài nguyên. | 23/06/2026 | 23/06/2026 | [CloudFormation Resource Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)<br>[Intrinsic Functions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html) |
| Thứ 4 | - Nâng cao kỹ năng tự động hóa bằng AWS CLI scripting trong môi trường Linux Bash.<br>- Sử dụng tham số `--query` với JMESPath để trích xuất thuộc tính tài nguyên (VPC ID, Instance IP).<br>- Viết bash script tự động kiểm tra trạng thái tài nguyên AWS và xuất báo cáo định dạng JSON. | 24/06/2026 | 24/06/2026 | [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html)<br>[Filtering AWS CLI Output](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-filter.html) |
| Thứ 5 | - Mở rộng CloudFormation Template để khai báo thêm Security Groups, EC2 Instance và RDS DBInstance.<br>- Tìm hiểu cơ chế CloudFormation Stack Events, Rollback khi xảy ra lỗi tạo tài nguyên.<br>- Học cách sử dụng tính năng Drift Detection để phát hiện các thay đổi thủ công ngoài luồng so với code khai báo ban đầu. | 25/06/2026 | 25/06/2026 | [Stack Management](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacks.html)<br>[Detecting Drift](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-stack-drift.html) |
| Thứ 6 | - **Thực hành & Tự động hóa:**<br>- Thực hiện lệnh `aws cloudformation create-stack` triển khai toàn bộ Stack hạ tầng 2-tier qua CLI.<br>- Kiểm tra tiến trình khởi tạo bằng `aws cloudformation describe-stack-events`.<br>- Xác minh hệ thống khởi tạo thành công và lấy thông tin URL ứng dụng từ phần Outputs.<br>- Tổng hợp mã nguồn YAML và lưu trữ tài liệu kết quả thực hành. | 26/06/2026 | 26/06/2026 | [AWS CLI CloudFormation](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudformation/index.html)<br>[Deploying Stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stacks.html) |

### Kết quả đạt được tuần 6

* Nắm vững tư duy Hạ tầng dưới dạng Mã nguồn (IaC), hiểu rõ khả năng tái sử dụng, nhất quán và giảm thiểu sai sót của CloudFormation.
* Thành thạo cú pháp YAML khai báo CloudFormation Template và cách kết hợp các Intrinsic Functions (`!Ref`, `!Sub`, `!GetAtt`).
* Tự tay xây dựng hoàn chỉnh một CloudFormation Template khai báo VPC, Subnets, Route Tables, Internet Gateway, Security Groups, EC2 và RDS.
* Làm chủ kỹ thuật trích xuất dữ liệu bằng AWS CLI kết hợp JMESPath Query (`--query`), hỗ trợ tự động hóa vận hành.
* Hiểu sâu về vòng đời Stack, cơ chế tự động Rollback bảo vệ hệ thống khi triển khai lỗi và tính năng Drift Detection.
* Thực hiện thành công việc khởi tạo toàn bộ hạ tầng đám mây 2-tier chỉ bằng một câu lệnh `aws cloudformation create-stack` trên CLI.
* Đã lưu trữ mã nguồn tệp `template.yaml` trong repository và hoàn thành chụp ảnh kiểm thử tiến trình tạo Stack.
