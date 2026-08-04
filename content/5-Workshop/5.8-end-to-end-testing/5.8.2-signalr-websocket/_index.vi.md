---
title: "Kiểm thử WebSocket (SignalR)"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.8.2. </b> "
---


Tính năng "Real-time Notification" của Snaptics yêu cầu kết nối 2 chiều liên tục (WebSocket). ALB của AWS mặc định hỗ trợ WebSocket rất tốt mà không cần cấu hình thêm.

### Các bước Test:

**1. Kết nối Client**
- Mở một tab trình duyệt khác (Hoặc dùng Postman WebSocket Client).
- Kết nối tới đường dẫn Hub của SignalR trên server của bạn:
  `ws://<ALB_DNS_NAME>/hubs/notification?access_token=<TOKEN>`
- Nếu kết nối báo Status `101 Switching Protocols`, tức là kết nối WebSocket thành công!

**2. Gửi sự kiện từ Server**
- Quay lại giao diện Swagger, gọi Endpoint `POST /api/Transactions` để tạo một giao dịch bằng tay (VD: Chi tiêu 50.000đ ăn sáng).
- Lập tức chuyển sang tab WebSocket Client đang mở.
- Bạn sẽ thấy Server chủ động đẩy (Push) một cục JSON về cho Client với nội dung:
  `{"type": "NEW_TRANSACTION_ADDED", "amount": 50000, "message": "Giao dịch mới đã được lưu!"}`

Điều này chứng minh ALB đang hoạt động định tuyến chính xác cả luồng HTTP và luồng WebSocket vào các container `ECS Fargate`. Kiến trúc được thiết kế theo hướng có khả năng mở rộng thông qua ECS Service Auto Scaling.
