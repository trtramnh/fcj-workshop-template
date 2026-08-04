---
title: "Kiểm thử API (Swagger)"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.8.1. </b> "
---


Nếu bạn cấu hình biến môi trường `ASPNETCORE_ENVIRONMENT = Development` trên ECS, giao diện Swagger UI sẽ được bật mặc định.

Truy cập trình duyệt thông qua DNS của ALB:
`http://<ALB_DNS_NAME>/swagger`

### Các bước Test Thực tế:

**1. Đăng ký và Đăng nhập (Auth)**
- Tìm Endpoint `POST /api/Auth/register`, nhập body tạo một tài khoản mới.
- Tìm Endpoint `POST /api/Auth/login`, hệ thống sẽ gọi xuống SQL Server (RDS), xác thực mật khẩu, lấy JWT Key từ Parameter Store và trả về một chuỗi `token`.
- Click nút **Authorize** ổ khóa màu xanh lá cây trên cùng của Swagger, nhập `Bearer <TOKEN>` để xác thực các request tiếp theo.

**2. Test Upload Hóa đơn & AI**
- Tìm Endpoint `POST /api/Transactions/scan-receipt`.
- Chọn file hình ảnh hóa đơn từ máy tính và thực thi.
- **Xác thực kết quả:**
  1. Mở AWS Console, vào **S3**, bạn sẽ thấy ảnh hóa đơn đã nằm trong bucket `s3-bucket-snaptics`.
  2. Mở **SQS**, bạn sẽ thấy có 1 Message nhảy lên (Nếu backend cấu hình đẩy qua hàng đợi thay vì gọi thẳng).
  3. Bật màn hình Swagger lên, bạn sẽ nhận được chuỗi JSON trả về chi tiết giá tiền (Total), Tên cửa hàng (Merchant) được bóc tách chuẩn xác bởi Azure Document Intelligence!
