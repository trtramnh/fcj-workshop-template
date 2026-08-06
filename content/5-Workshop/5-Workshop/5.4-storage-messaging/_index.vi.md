---
title: "Database, Storage & Secrets"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---


Ở bài này, chúng ta sẽ xây dựng tầng lưu trữ. Vì hệ thống hướng tới môi trường Production, ta sẽ loại bỏ các DB thông thường và file cấu hình thô sơ để thay thế bằng SQL Server và AWS Systems Manager Parameter Store.

## 1. Cơ sở dữ liệu Cốt lõi (Amazon RDS for SQL Server)

Snaptics yêu cầu một DB không bao giờ được phép sập (High Availability). SQL Server tự động sao chép dữ liệu của bạn ra nhiều Availability Zones khác nhau.

### A. Khởi tạo DB Subnet Group
- Vào **Amazon RDS ➔ Subnet groups ➔ Create DB subnet group**.
- **Name:** `snaptics-db-subnet-group`.
- **VPC:** Chọn `snaptics-vpc`.
- **Subnets:** Ở mục này phải chọn cẩn thận! Chọn 2 Availability Zones và chỉ tick vào **2 mạng Private Subnets** (`10.0.3.0/24` và `10.0.4.0/24`).

### B. Khởi tạo Cụm SQL Server Cluster
- Vào **RDS ➔ Databases ➔ Create database**.
- **Engine options:** Bắt buộc chọn **SQL Server**.
- **Choose a database creation method:** Chọn **Full configuration**.
- **Templates:** Chọn **Dev/Test**.
- **Settings:**
  - DB cluster identifier: `snaptics-sql-server`
  - Master username: `admin`
  - Master password: Gõ một mật khẩu thật mạnh (Ví dụ `SnapticsAurora2024!`).
- **Instance configuration:**
  - Chọn **Burstable classes (includes t classes)**.
  - Instance type: `db.t3.micro`.
- **Storage:**
  - Storage type: **General purpose SSD (gp3)**.
  - Allocated Storage: `200`.
  - Provisioned IOPS: `3000`.
  - Storage throughput: `125`.
- **Connectivity:**
  - Compute resource: chọn **Don't connect to an EC2 compute resource**.
  - VPC: Chọn `snaptics-vpc`.
  - **Public access: No** (Rất quan trọng, để No để hacker không thể quét ra cổng DB của bạn).
  - VPC security group: chọn **Choose existing**, sau đó chọn `default`.
  - Availability Zone: **No preference**.
- Các mục còn lại giữ nguyên, sau đó bấm **Create database**. Cụm SQL Server sẽ mất khoảng 15 phút để tạo. Sau khi xong, copy lấy chuỗi **Writer Endpoint**.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.4-storage-messaging/aurora_and_rds_create_db.jpg" >
  </div>

## 2. Kho lưu trữ Hóa đơn (Amazon S3)

Vì ở bài trước chúng ta đã tạo **VPC Gateway Endpoint**, Code C# chạy trong ECS giờ đây sẽ đẩy thẳng file ảnh hóa đơn vào S3 xuyên qua mạng nội bộ, tốc độ cực cao và hoàn toàn miễn phí băng thông.

- Vào **Amazon S3 ➔ Create bucket**.
- **Bucket name:** `s3-bucket-snaptics` (Phải gõ thêm số linh tinh vì tên S3 là duy nhất toàn cầu).
- **Region:** `ap-southeast-1`.
- **Block Public Access:** Giữ trạng thái **ON** để bảo mật tuyệt đối ảnh hóa đơn tài chính của người dùng.
- **Cấu hình CORS (Tab Permissions):** Cho phép Frontend trên AWS Amplify gọi trực tiếp vào S3 thông qua URL ký sẵn (Pre-signed URL).
```json
[
    {
        "AllowedMethods": [ "GET", "PUT", "POST" ],
        "AllowedOrigins": [ "*" ],
        "AllowedHeaders": [ "*" ]
    }
]
```
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.4-storage-messaging/amazon_s3_create.jpg" >
  </div>

## 3. Két sắt Bí mật (AWS Systems Manager Parameter Store)

TUYỆT ĐỐI KHÔNG lưu mật khẩu SQL Server hay API Key của AI vào file `appsettings.json` và đẩy lên GitHub! Chúng ta sẽ sử dụng **AWS Systems Manager Parameter Store** để lưu trữ các thông tin này an toàn theo dạng phân cấp.

- Vào **AWS Systems Manager ➔ Parameter Store ➔ Create parameter**.
- Tạo lần lượt các parameter sau. Với các chuỗi mật khẩu/khóa, hãy chọn Type là **SecureString** để mã hóa (AWS tự động dùng KMS), còn các chuỗi bình thường chọn Type là **String**:

  - `/Snaptics/Production/AWS/AccessKey` (Type: **SecureString**)
  - `/Snaptics/Production/AWS/SecretKey` (Type: **SecureString**)
  - `/Snaptics/Production/AiSettings/AzureDocIntelKey` (Type: **SecureString**)
  - `/Snaptics/Production/AiSettings/GeminiApiKey` (Type: **SecureString**)
  - `/Snaptics/Production/AwsSns/TopicArn` (Type: **String**)
  - `/Snaptics/Production/ConnectionStrings/DefaultConnection` (Type: **SecureString**)
  - `/Snaptics/Production/EmailSettings/Email` (Type: **SecureString**)
  - `/Snaptics/Production/EmailSettings/Password` (Type: **SecureString**)
  - `/Snaptics/Production/TokenKey` (Type: **SecureString**)

Ở trong code `.NET`, nhờ thư viện AWS SDK, ứng dụng sẽ tự động tải các thông số này theo đường dẫn `/Snaptics/Production/*` đè lên `appsettings.json`, giữ cho mã nguồn của bạn hoàn toàn sạch sẽ và an toàn.