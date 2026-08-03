---
title: "Worklog Tuần 3"
date: 2026-06-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3

* Nắm vững khái niệm và nguyên lý hoạt động của dịch vụ lưu trữ đối tượng Amazon S3 (Buckets, Objects, Key-Value store).
* Phân biệt các lớp lưu trữ (Storage Classes) của S3 như S3 Standard, S3 Intelligent-Tiering, S3 Standard-IA, và Amazon Glacier để tối ưu chi phí.
* Học cách bảo mật dữ liệu S3 thông qua Block Public Access, Bucket Policy, Access Control List (ACL) và Server-Side Encryption (SSE-S3, SSE-KMS).
* Nắm vững các khái niệm cốt lõi của AWS IAM (IAM Users, IAM Groups, IAM Roles, IAM Policies) và nguyên tắc đặc quyền tối thiểu (Least Privilege).
* Tích hợp AWSSDK.S3 vào ứng dụng Backend C#/.NET, triển khai kỹ thuật sinh Pre-signed URL, cấu hình CORS và S3 Lifecycle Policy.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Tìm hiểu tổng quan về Amazon S3 và cấu trúc lưu trữ dạng Object.<br>- Nghiên cứu các thành phần: Bucket, Object, Key, Metadata và Versioning.<br>- Phân tích các gói lưu trữ S3 Storage Classes (Standard, Intelligent-Tiering, Standard-IA, Glacier) và bài toán tối ưu chi phí lưu trữ. | 01/06/2026 | 01/06/2026 | [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)<br>[S3 Storage Classes](https://aws.amazon.com/s3/storage-classes/) |
| Thứ 3 | - Tìm hiểu các cơ chế bảo mật trên Amazon S3.<br>- Thực hành cấu hình Block Public Access để ngăn chặn vô tình lộ lọt dữ liệu ra Internet.<br>- Viết Bucket Policy JSON để phân quyền truy cập chi tiết cho tài nguyên S3.<br>- Cấu hình mã hóa dữ liệu at-rest bằng Server-Side Encryption (SSE-S3 và SSE-KMS). | 02/06/2026 | 02/06/2026 | [Amazon S3 Security](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html)<br>[S3 Bucket Policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html) |
| Thứ 4 | - Nghiên cứu dịch vụ AWS IAM (Identity and Access Management).<br>- Phân biệt IAM User, IAM Group, IAM Role và IAM Policy.<br>- Học nguyên tắc đặc quyền tối thiểu (Least Privilege) trong quản trị bảo mật Đám mây.<br>- Thực hành tạo IAM User dành riêng cho backend application và gắn Policy chỉ cấp quyền `s3:GetObject` và `s3:PutObject`. | 03/06/2026 | 03/06/2026 | [AWS IAM Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)<br>[IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) |
| Thứ 5 | - Học cách tích hợp AWSSDK.S3 vào dự án backend C#/.NET.<br>- Xây dựng S3Service trong mã nguồn để xử lý các hàm `PutObjectAsync`, `GetObjectAsync`, `DeleteObjectAsync`.<br>- Tìm hiểu và triển khai kỹ thuật Pre-signed URL giúp truy xuất file an toàn thời hạn 15–30 phút.<br>- Quản lý thông tin xác thực (Access Key, Secret Key) an toàn qua Environment Variables. | 04/06/2026 | 04/06/2026 | [AWS SDK for .NET S3 Guide](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/s3-index.html)<br>[Pre-signed URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ShareObjectPreSignedURL.html) |
| Thứ 6 | - **Thực hành & Khắc phục sự cố:**<br>- Phát hiện và xử lý lỗi CORS khi ứng dụng frontend gọi Pre-signed URL tải file.<br>- Thêm cấu hình CORS Policy chuẩn xác trên S3 Bucket (cho phép AllowedOrigins, AllowedMethods).<br>- Cấu hình S3 Lifecycle Policy để tự động xóa các file tạm trong thư mục `temp/` sau 7 ngày để tối ưu chi phí.<br>- Chụp ảnh minh chứng và lưu lại kết quả thực hành. | 05/06/2026 | 05/06/2026 | [S3 CORS Configuration](https://docs.aws.amazon.com/AmazonS3/latest/userguide/cors.html)<br>[S3 Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html) |

### Kết quả đạt được tuần 3

* Hiểu rõ bản chất của Amazon S3 là dịch vụ Object Storage và nắm vững các khái niệm Bucket, Object, Key, Metadata, Versioning.
* Phân biệt được các lớp lưu trữ S3 Storage Classes và biết cách lựa chọn lớp lưu trữ phù hợp để tối ưu chi phí.
* Nắm vững kiến thức bảo mật S3, thực hành kích hoạt Block Public Access, cấu hình Bucket Policy và mã hóa dữ liệu bằng SSE-S3/SSE-KMS.
* Nắm vững các thành phần cốt lõi của AWS IAM (User, Group, Role, Policy) và tuân thủ tuyệt đối nguyên tắc đặc quyền tối thiểu (Least Privilege).
* Khởi tạo IAM User chuyên biệt cho ứng dụng Backend với chính sách phân quyền JSON hạn chế truy cập chỉ ở S3 Bucket mục tiêu.
* Tích hợp thành công thư viện AWSSDK.S3 vào dự án C#/.NET Backend, hỗ trợ đầy đủ các thao tác upload, download, delete file.
* Triển khai thành công kỹ thuật Pre-signed URL giúp chia sẻ tài liệu an toàn trong khoảng thời gian có hạn mà không mở công khai S3 Bucket.
* Khắc phục thành công sự cố CORS trình duyệt khi gọi Pre-signed URL và thiết lập S3 Lifecycle Policy để tự động dọn dẹp các tệp tin tạm thời.
