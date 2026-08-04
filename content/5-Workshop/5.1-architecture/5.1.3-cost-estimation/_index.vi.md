---
title: "Phân tích Chi phí (Cost Estimation)"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.1.3. </b> "
---


Việc hiểu rõ cấu trúc chi phí (Billing) trên AWS là kỹ năng sống còn của một Cloud Engineer. Kiến trúc của Snaptics bao gồm nhiều thành phần Managed Services có thể phát sinh chi phí hàng tháng khá lớn nếu không tối ưu. 

Dưới đây là bảng dự toán chi phí hàng tháng cho một môi trường **Production mức cơ bản** tại Region `ap-southeast-1` (Singapore), giả sử hệ thống phục vụ khoảng 10,000 lượt request mỗi tháng.

| Dịch vụ AWS | Cấu hình sử dụng | Chi phí dự kiến (USD/tháng) | Lưu ý |
| :--- | :--- | :--- | :--- |
| **Amazon VPC (NAT Gateway)** | 1 NAT Gateway, 10 GB Data Processed | **~$35.00** | NAT Gateway tính phí theo giờ ($0.059/hr), là chi phí cố định nặng nhất. |
| **Amazon RDS (SQL Server)** | `db.t3.micro`, Single-AZ, 20GB Storage | **~$22.00** | Bản Express. Nếu dùng Multi-AZ, chi phí sẽ nhân đôi (~$44). |
| **Application Load Balancer** | 1 ALB chạy liên tục 730 giờ | **~$18.00** | Tính thêm phụ phí theo số lượng kết nối (LCU). |
| **Amazon ECS Fargate** | 1 Task (0.25 vCPU, 1 GB RAM) chạy liên tục | **~$10.00** | Chi phí tính trên lượng RAM và CPU được cấp phát. Có thể dùng Spot Instance để giảm 70% giá. |
| **Amazon S3** | 50 GB Storage, 10,000 requests | **~$1.50** | Rất rẻ, tính theo dung lượng GB thực lưu. |
| **Amazon SQS / SNS** | Dưới 1 triệu requests | **~$0.00** | Free Tier của AWS cấp cho 1 triệu tin nhắn miễn phí mỗi tháng. |
| **Parameter Store (Standard)** | Lưu < 100 Secrets | **~$0.00** | Free. (Bản Advanced mới tính phí). |
| **Tổng cộng (Ước tính)** | | **~$86.50** | |

> [!WARNING]
> Mức giá trên chỉ là **Ước tính**. Trên môi trường thực tế, nếu bạn để hệ thống chạy quên không tắt (đặc biệt là NAT Gateway và RDS), tài khoản AWS của bạn sẽ bị trừ khoảng $80 - $90 vào cuối tháng. Hãy luôn nhớ thực hiện phần **Dọn dẹp tài nguyên (Cleanup)** ở cuối bài Workshop!

### Các API Trí tuệ Nhân tạo Ngoại vi

Chi phí cho AI không nằm trong AWS mà thanh toán cho Google và Microsoft:
- **Google Gemini API:** Hiện tại Google cung cấp gói Free Tier khá hào phóng cho bản 1.5 Flash (15 RPM). Phù hợp để làm lab miễn phí.
- **Azure Document Intelligence:** Tính phí khoảng `$10` cho mỗi 1,000 trang hóa đơn được phân tích thành công (Pay-as-you-go). Bản Free Tier (F0) cho phép 500 trang/tháng miễn phí.
