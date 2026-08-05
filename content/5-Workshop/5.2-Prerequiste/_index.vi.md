---
title: "Chuẩn bị Môi trường"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---


Để triển khai kiến trúc Enterprise khổng lồ của Snaptics một cách mượt mà, bạn cần thiết lập kỹ lưỡng các quyền hạn trên Cloud. Khác với cách làm thủ công cũ, chúng ta sẽ dùng CI/CD hoàn toàn tự động, do đó bạn không cần phải cài đặt Docker hay AWS CLI ở máy tính local nữa. Trọng tâm bây giờ là **Phân quyền**.

## 1. Các Tài khoản & APIS

Trước khi đụng vào AWS, hãy chuẩn bị sẵn 3 thứ sau:

1. **Tài khoản GitHub:** Chúng ta sẽ sử dụng **GitHub Actions** làm CI/CD Pipeline. Bạn cần một tài khoản GitHub để Fork (sao chép) mã nguồn Snaptics về kho của mình.
2. **Google Gemini API Key:** Cần thiết cho tính năng AI tư vấn tài chính. Đăng ký miễn phí tại [Google AI Studio](https://aistudio.google.com/app/apikey).
3. **Azure Document Intelligence Key:** Dùng để quét hóa đơn (OCR). Đăng nhập vào [Azure Portal](https://portal.azure.com/), tạo tài nguyên **Document Intelligence**, sau đó copy **Endpoint URL** và **Key 1**.

## 2. Tạo IAM User cho GitHub Actions

Vì GitHub Actions đóng vai trò là một "con robot" tự động đẩy code lên AWS thay bạn, nó cần được cấp quyền truy cập vào tài khoản AWS của bạn bằng khóa API.

### A. Khởi tạo User
1. Vào AWS Console, mở dịch vụ **IAM ➔ Users ➔ Create user**.
2. Đặt tên: `github-actions-snaptics`.
3. **KHÔNG** tick chọn ô "Provide user access to the AWS Management Console" (Con bot này chỉ gọi API, không cần đăng nhập giao diện web).
4. Bấm Next. Ở mục Permissions, chọn **Attach policies directly**.
5. Gắn quyền `AdministratorAccess`. *(Lưu ý: Trong dự án công ty thực tế, bạn chỉ nên cấp quyền ECS/ECR/S3 vừa đủ dùng. Ở workshop này, ta dùng Admin để rút ngắn rào cản kỹ thuật cấu hình Pipeline).*
6. Bấm Create user.

### B. Cấp phát Khóa (Access Keys)
1. Bấm vào user `github-actions-snaptics` vừa tạo.
2. Chuyển sang tab **Security credentials**.
3. Cuộn xuống phần **Access keys**, bấm **Create access key**.
4. Chọn Use case là **Command Line Interface (CLI)**, xác nhận và bấm Next.
5. Copy hai chuỗi **Access key ID** và **Secret access key**. **Hãy lưu 2 chuỗi này vào Notepad ngay lập tức!** Chút nữa ta sẽ phải dán nó vào GitHub.

## 3. Phân quyền IAM Roles cho ECS

Khi mã nguồn C# .NET chạy bên trong Container trên ECS Fargate, nó cần quyền để kết nối tới S3, SQS và lấy mật khẩu DB từ Parameter Store. Thay vì nhúng Access Key vào code rất nguy hiểm, AWS sử dụng IAM Roles.

Vào **IAM ➔ Roles ➔ Create role** và tạo lần lượt 2 Role sau:

### A. ECS Task Execution Role (`ecsTaskExecutionRole`)
Role này cấp quyền cho nền tảng phần cứng ECS để nó tự động tải Docker Image từ ECR và tạo Log trong CloudWatch.
- **Trusted Entity:** `Elastic Container Service Task`
- **Managed Policy:** Tìm và gắn `AmazonECSTaskExecutionRolePolicy`.
- Gắn thêm quyền `AmazonSSMReadOnlyAccess` để ECS có quyền đọc mật khẩu DB khi khởi động.

### B. Snaptics ECS Task Role (`snaptics-ecs-task-role`)
Role này cấp quyền cho **chính mã nguồn C#** của bạn.
- **Trusted Entity:** `Elastic Container Service Task`
- **Inline Policy (JSON):** Tạo một policy dạng JSON và dán đoạn code sau vào. Nó chỉ cho phép C# gọi S3, SQS, SNS và đọc Parameter Store.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowParameterStore",
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameter",
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
> Việc cấu hình Role khắt khe như thế này là tuân thủ nguyên tắc **Đặc quyền tối thiểu (Least Privilege)** của các hệ thống ngân hàng. Giả sử hacker có chiếm được quyền điều khiển Container của bạn, hắn cũng không thể xóa Database vì Role này hoàn toàn không có quyền đụng vào RDS!