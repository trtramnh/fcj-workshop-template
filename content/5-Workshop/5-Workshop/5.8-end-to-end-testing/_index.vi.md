---
title: "Kiểm thử Hệ thống (E2E Testing)"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---


Chúc mừng bạn đã dựng xong khối kiến trúc Enterprise đồ sộ! Giờ là lúc kiểm chứng xem mọi mảnh ghép (CloudFront ➔ ALB ➔ ECS ➔ SQL Server/S3/SQS) có đang mượt mà vắt chéo nhau không.

## 1. Kiểm chứng Mạng Phân phối (CloudFront)

Lần này, ta KHÔNG truy cập qua DNS của ALB nữa. Hãy mở trình duyệt và gõ **Domain của CloudFront** (Ví dụ `https://d1234abcd.cloudfront.net/swagger` hoặc Domain của bạn trên Route 53).

1. **Test Load Tĩnh:** Truy cập trang Swagger bình thường, bạn sẽ thấy nó load nhanh vì CloudFront đã tối ưu đường truyền ở các trạm phát sóng (Edge location).
  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.8-end-to-end-testing/cloudfront.jpg" >
  </div>

## 2. Kiểm thử API & Trí tuệ Nhân tạo (Swagger)

1. **Đăng nhập (Auth):** Dùng Endpoint `/api/Auth/register` rồi `/api/Auth/login`. Bước này chứng minh ECS Container đã đọc được mật khẩu từ **Parameter Store** và kết nối thành công tới **SQL Server Database**!
2. **Upload Hóa đơn:** Dùng Endpoint `/api/Transaction/from-bill` tải lên một bức ảnh hóa đơn thật.
3. **Xác thực Luồng Data:**
   - Mở **S3**, bức ảnh đã nằm đó (Chứng minh **VPC Gateway Endpoint** xuyên mạng nội bộ thành công).
   - Mở **SQS**, một message nhảy lên ở hàng đợi `snaptics-ai-queue`.
   - Mở **CloudWatch Logs**, bạn sẽ thấy log của Hangfire đang bốc message ra và gọi sang Azure OCR.
   - Quay lại Swagger, bạn nhận được chuỗi JSON bóc tách chính xác tên cửa hàng!

## 3. Kiểm thử WebSocket xuyên tường (SignalR)

Để xem Real-time Notification có đâm xuyên qua được CloudFront và ALB không.

1. Dùng công cụ Client kết nối tới: `wss://<CLOUDFRONT_DOMAIN>/hubs/notification?access_token=<TOKEN>`
2. Kiểm tra log thấy báo `101 Switching Protocols` là thành công.
3. Mở song song trang Swagger, gọi Endpoint tạo 1 giao dịch thủ công.
4. Ngay lập tức màn hình WebSocket Client nhận được một chuỗi JSON đẩy về: `{"type": "NEW_TRANSACTION_ADDED"}`. 

Điều này chứng minh CloudFront và ALB đã làm rất tốt việc Upgrade giao thức HTTP lên WebSocket và giữ đường hầm kết nối liên tục (Persistent tunnel) thẳng tới lõi Fargate bên trong mạng Private!