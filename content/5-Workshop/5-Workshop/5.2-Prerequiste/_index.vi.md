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

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iamuser_create_1.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_permission.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_create_2.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_create_complete.jpg" >
  </div>

### B. Cấp phát Khóa (Access Keys)
1. Bấm vào user `github-actions-snaptics` vừa tạo.
2. Chuyển sang tab **Security credentials**.
3. Cuộn xuống phần **Access keys**, bấm **Create access key**.
4. Chọn Use case là **Command Line Interface (CLI)**, xác nhận và bấm Next.
5. Copy hai chuỗi **Access key ID** và **Secret access key**. **Hãy lưu 2 chuỗi này vào Notepad ngay lập tức!** Chút nữa ta sẽ phải dán nó vào GitHub.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_security_credentials.jpg" >
  </div>

<div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_security_credentials_2.jpg" >
  </div>

<div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/iam_user_security_credentials_3.jpg" >
  </div>

## 3. Phân quyền IAM Roles cho ECS

Khi mã nguồn C# .NET chạy bên trong Container trên ECS Fargate, nó cần quyền để kết nối tới S3, SQS, SNS và lấy mật khẩu DB từ Parameter Store. Thay vì nhúng Access Key vào code rất nguy hiểm, AWS sử dụng IAM Roles. Chúng ta cần tạo **2 Role** riêng biệt.

### A. Tạo ECS Task Execution Role (`ecsTaskExecutionRole`)

1. Mở **IAM ➔ Roles ➔ Create role**.
2. Bấm vào **Choose a service or use case**.
3. Tìm và chọn **Elastic Container Service**.
4. Ở phần **Use case**, chọn chính xác **Elastic Container Service Task**. Không chọn "EC2 Role for Elastic Container Service".
5. Bấm **Next**.

**Ở bước Add permissions:**

6. Tìm `AmazonECSTaskExecutionRolePolicy` và tick chọn policy đó.
7. Nếu bạn đang đưa mật khẩu DB từ Parameter Store vào mục **Secrets** của ECS Task Definition, tìm và tick thêm `AmazonSSMReadOnlyAccess`.
8. Bấm **Next**.

**Đặt tên và tạo role:**

9. Đặt tên role là `ecsTaskExecutionRole`.
10. Bấm **Create role**.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3a_ecs_role_create.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3a_ecs_role_create_1.jpg" >
  </div>

> [!NOTE]
> AWS xác nhận task execution role dùng để ECS **kéo image từ ECR**, **ghi log vào CloudWatch** và **lấy Parameter Store/Secrets Manager** khi task definition tham chiếu secret.
>
> `AmazonSSMReadOnlyAccess` khá rộng. Có thể dùng trước để triển khai, sau đó thay bằng inline policy chỉ đọc đúng đường dẫn `/Snaptics/Production/*`.

### B. Tạo Snaptics ECS Task Role (`snaptics-ecs-task-role`)

1. Quay lại **IAM ➔ Roles ➔ Create role**.
2. **Trusted entity type:** AWS service.
3. **Service or use case:** Elastic Container Service.
4. **Use case:** Elastic Container Service Task.
5. Bấm **Next**.
6. Tại **Add permissions**, chưa cần chọn policy nào; bấm **Next**.
7. Đặt tên role là `snaptics-ecs-task-role`.
8. Bấm **Create role**.

**Sau khi role được tạo:**

9. Mở role `snaptics-ecs-task-role`.
10. Chọn tab **Permissions**.
11. Chọn **Add permissions ➔ Create inline policy**.
12. Chuyển sang tab **JSON**.
13. Xóa nội dung cũ và dán policy dưới đây. Nên thay `<ACCOUNT_ID>` bằng AWS Account ID của bạn.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowParameterStore",
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameter",
                "ssm:GetParameters",
                "ssm:GetParametersByPath"
            ],
            "Resource": "arn:aws:ssm:ap-southeast-1:<ACCOUNT_ID>:parameter/Snaptics/Production/*"
        },
        {
            "Sid": "AllowS3BucketListing",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::s3-bucket-snaptics"
        },
        {
            "Sid": "AllowS3Objects",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::s3-bucket-snaptics/*"
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

14. Bấm **Next**.
15. Policy name nhập `SnapticsRuntimePolicy`.
16. Bấm **Create policy**.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_1.jpg" >
  </div>

<div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_2.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_3.jpg" >
  </div>  

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_4.jpg" >
  </div>  

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_5.jpg" >
  </div>  

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3b_ecs_role_create_6.jpg" >
  </div>  


> [!NOTE]
> Task role chính là quyền mà **mã C# bên trong container** sử dụng để gọi S3, SQS, SNS hoặc Parameter Store; nó tách biệt với quyền ECS dùng để khởi động container.

### C. Khi tạo ECS Task Definition

Khi tạo ECS Task Definition, gán đúng hai trường:

| Field | Value |
|-------|-------|
| **Task role** | `snaptics-ecs-task-role` |
| **Task execution role** | `ecsTaskExecutionRole` |

Phân biệt như sau:

- `ecsTaskExecutionRole` ➔ ECS **kéo image**, **tạo log**, **inject secret** khi container khởi động.
- `snaptics-ecs-task-role` ➔ **Code C# đang chạy** gọi S3, SQS, SNS, SSM.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3c_ecs_assign_role_1.jpg" >
  </div>

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3c_ecs_assign_role_2.jpg" >
  </div> 

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.2-Prerequisite/3c_ecs_assign_role_3.jpg" >
  </div>  

> [!TIP]
> Nếu Parameter Store sử dụng **SecureString với customer-managed KMS key**, role thực sự đọc parameter còn cần thêm quyền `kms:Decrypt` đối với KMS key đó. Nếu mật khẩu được cấu hình trong mục **Secrets** của task definition thì quyền này thuộc **execution role**; nếu C# tự gọi SSM bằng AWS SDK thì thuộc **task role**.