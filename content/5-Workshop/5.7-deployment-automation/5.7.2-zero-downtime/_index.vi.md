---
title: "Cơ chế Zero-Downtime Deployment"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---


Lệnh `--force-new-deployment` trong script `deploy.ps1` kích hoạt một cơ chế cực kỳ tinh vi của AWS ECS, thường được biết đến với tên gọi **Rolling Update (Cập nhật cuốn chiếu)**.

Nhờ cơ chế này, hệ thống của bạn hoàn toàn không bị "chết" (downtime) trong lúc cập nhật phiên bản mới. Quá trình diễn ra như sau:

1. **Khởi tạo Task mới:** ECS giữ nguyên các Task cũ (đang chạy code cũ). Đồng thời, nó kéo image mới từ ECR về và tạo ra các Task mới.
2. **Kiểm tra Sức khỏe (Health Check):** Các Task mới bắt đầu khởi động (chạy `dotnet run`). ALB sẽ ping liên tục vào cổng 8080 của chúng.
3. **Chuyển luồng (Traffic Routing):** Khi ALB xác nhận Task mới đã khỏe (Healthy 200 OK), nó mới bắt đầu hướng (Forward) request của người dùng vào đây.
4. **Hủy Task cũ (Draining):** ALB ngừng gửi request mới vào các Task cũ, đợi chúng xử lý xong các request đang dở dang. Cuối cùng, ECS ra lệnh tiêu diệt (SIGTERM) các Task cũ.

> [!NOTE]
> Kết quả là: Người dùng đang sử dụng App sẽ không hề biết hệ thống vừa được cập nhật, không có request nào bị rớt (502 Bad Gateway), trải nghiệm xuyên suốt 100%.
