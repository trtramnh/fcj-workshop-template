---
title: "Phân quyền IAM Roles"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---


Để ứng dụng .NET chạy trên container ECS có thể giao tiếp được với các dịch vụ khác của AWS (như gửi file vào S3, đọc biến môi trường từ Parameter Store, thả message vào SQS), chúng ta cần cấp phát các quyền truy cập hợp lệ (IAM Policies) thay vì nhúng trực tiếp API Key của AWS vào source code.

Trong AWS Console, truy cập vào **IAM ➔ Roles ➔ Create role** và tạo 2 Role với cấu hình chi tiết dưới đây:

### 1. ECS Task Execution Role (`ecsTaskExecutionRole`)

Role này cấp quyền cho nền tảng ECS. Nhiệm vụ chính của nó là tự động pull Docker image từ Elastic Container Registry (ECR) và tạo luồng log (Log Streams) vào Amazon CloudWatch.

- **Trusted Entity:** `Elastic Container Service Task`
- **Managed Policy (Được AWS tạo sẵn):** 
  - `AmazonECSTaskExecutionRolePolicy`

Mã JSON mô tả chính xác quyền của Role này:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        }
    ]
}
```

### 2. Snaptics ECS Task Role (`snaptics-ecs-task-role`)

Role này cấp quyền cho **chính đoạn code C# .NET của bạn** đang thực thi bên trong container.

- **Trusted Entity:** `Elastic Container Service Task`
- **Permissions:** Tạo một Inline Policy mới và dán đoạn JSON sau vào. Nó cấp cho ứng dụng của bạn quyền Đọc cấu hình từ SSM Parameter Store, Đọc/Ghi dữ liệu lên bucket S3, và xử lý hàng đợi SQS, SNS.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowParameterStore",
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameters",
                "ssm:GetParametersByPath"
            ],
            "Resource": "arn:aws:ssm:ap-southeast-1:*:parameter/Snaptics/Production/*"
        },
        {
            "Sid": "AllowS3Storage",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::s3-bucket-snaptics",
                "arn:aws:s3:::s3-bucket-snaptics/*"
            ]
        },
        {
            "Sid": "AllowSQSSNS",
            "Effect": "Allow",
            "Action": [
                "sqs:SendMessage",
                "sqs:ReceiveMessage",
                "sqs:DeleteMessage",
                "sns:Publish"
            ],
            "Resource": "*"
        }
    ]
}
```

> [!TIP]
> Bằng cách giới hạn `Resource` thành các tên cụ thể (như `s3-bucket-snaptics` thay vì `*`), kiến trúc của chúng ta hoàn toàn tuân thủ nguyên tắc **Least Privilege (Quyền hạn tối thiểu)** của các hệ thống doanh nghiệp lớn.
