---
title: "Amazon S3 cho Upload"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---


<!-- TODO: ChÃ¨n áº£nh mÃ n hÃ¬nh danh sÃ¡ch Amazon S3 Bucket chá»©a áº£nh hÃ³a Ä‘Æ¡n vÃ o Ä‘Ã¢y -->
![S3 Bucket](/images/5-Workshop/placeholder-s3.png)
Mỗi khi người dùng Snaptics tải lên một hóa đơn để hệ thống AI phân tích, bức ảnh đó không được lưu vào Database hay ổ cứng của Container, mà được đẩy lên S3.

### 1. Tạo Bucket S3
- Mở **Amazon S3 ➔ Create bucket**.
- **Bucket name:** `s3-bucket-snaptics-123` (Thêm một vài số ngẫu nhiên vì tên S3 phải độc nhất toàn cầu).
- **Region:** `ap-southeast-1`.
- **Block Public Access settings for this bucket:** Đảm bảo chọn **Block all public access** (Bật). Chúng ta muốn giữ file bảo mật tuyệt đối.

### 2. Cấu hình CORS (Cross-Origin Resource Sharing)
Nếu bạn dự định xây dựng Frontend Web (như React/Vue) gọi trực tiếp lên S3 thông qua *Pre-signed URL* do API cấp, bạn phải mở CORS.
- Vào Bucket vừa tạo ➔ Tab **Permissions**.
- Kéo xuống mục **Cross-origin resource sharing (CORS)** ➔ Bấm **Edit** và dán đoạn JSON sau:

```json
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "PUT",
            "POST"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    }
]
```
*(Trong thực tế, `AllowedOrigins` nên đổi thành domain frontend của bạn thay vì `*`).*
