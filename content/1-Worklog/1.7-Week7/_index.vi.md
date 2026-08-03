---
title: "Worklog Tuần 7"
date: 2026-06-29
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7

* Phân tích rủi ro bảo mật và hạn chế băng thông khi máy chủ EC2 truy cập Amazon S3 qua Public Internet hoặc NAT Gateway.
* Nắm vững nguyên lý hoạt động của Gateway VPC Endpoints (dành cho Amazon S3 và DynamoDB), cơ chế tích hợp Route Table và ưu điểm về chi phí (Miễn phí).
* Thực hành khởi tạo Gateway VPC Endpoint cho Amazon S3 theo đúng tài liệu Workshop chương 5.3 ("Truy cập đến S3 từ VPC").
* Cập nhật Route Table của Private Subnet để chuyển hướng toàn bộ lưu lượng tới Amazon S3 qua mạng nội bộ bảo mật của AWS.
* Thực thi kiểm tra truy cập S3 từ máy chủ EC2 nằm trong Private Subnet bị cô lập hoàn toàn (không có Internet Gateway và NAT Gateway).

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Nghiên cứu khái niệm VPC Endpoints và giải pháp kết nối riêng tư (Private Connectivity) tới các dịch vụ AWS.<br>- So sánh 2 loại điểm cuối: Gateway VPC Endpoint và Interface VPC Endpoint (AWS PrivateLink).<br>- Phân tích bài toán truy cập S3 từ VPC không cần cấp phát Public IP hay NAT Gateway. | 29/06/2026 | 29/06/2026 | [VPC Endpoints Guide](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html)<br>[Gateway Endpoints for S3](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-s3.html) |
| Thứ 3 | - Chuẩn bị môi trường thực hành cho Workshop theo hướng dẫn chương 5.2 ("Chuẩn bị").<br>- Khởi tạo VPC thực nghiệm, Public Subnet, Private Subnet và máy chủ EC2 trong Private Subnet không có kết nối Internet.<br>- Kiểm tra thử lệnh `aws s3 ls` từ EC2 Private Subnet và xác nhận bị hỏng kết nối (Timeout) do chưa có đường truyền. | 30/06/2026 | 30/06/2026 | [Workshop Chuẩn bị](5.2-Prerequiste/)<br>[AWS S3 CLI Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/index.html) |
| Thứ 4 | - Thực hành tạo Gateway VPC Endpoint cho dịch vụ `com.amazonaws.ap-southeast-1.s3` từ AWS Console.<br>- Lựa chọn VPC mục tiêu và gắn Gateway Endpoint vào Route Table đại diện cho Private Subnet.<br>- Quan sát sự thay đổi của Route Table: xuất hiện đường định tuyến mới trỏ Prefix List của S3 (`pl-xxxxxx`) tới Endpoint ID (`vpce-xxxxxx`). | 01/07/2026 | 01/07/2026 | [Workshop S3 từ VPC](5.3-S3-vpc/)<br>[Modifying Route Tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) |
| Thứ 5 | - **Thực hành kiểm thử kết nối:**<br>- Quay trở lại máy chủ EC2 trong Private Subnet và thực thi lại lệnh `aws s3 ls` và `aws s3 cp`.<br>- Xác nhận kết nối thành công tức thì với tốc độ cao mà lưu lượng hoàn toàn không đi ra ngoài Internet.<br>- Kiểm tra địa chỉ IP giải mã và xác nhận lưu lượng đi qua dải IP riêng của AWS. | 02/07/2026 | 02/07/2026 | [Workshop S3 từ VPC](5.3-S3-vpc/)<br>[Testing S3 Endpoint Connectivity](https://docs.aws.amazon.com/vpc/latest/privatelink/test-endpoints.html) |
| Thứ 6 | - Phân tích bảng so sánh hiệu năng và chi phí giữa NAT Gateway và Gateway VPC Endpoint khi làm việc với S3.<br>- Tổng hợp kết quả thực hành, trích xuất log lệnh CLI và chụp ảnh giao diện minh chứng cho báo cáo Workshop 5.3.<br>- Biên tập lại tài liệu thực hành và chuẩn bị kiến thức cho phần Interface Endpoint. | 03/07/2026 | 03/07/2026 | [Workshop Overview](5.1-Workshop-overview/)<br>[AWS PrivateLink Pricing](https://aws.amazon.com/privatelink/pricing/) |

### Kết quả đạt được tuần 7

* Phân tích rõ ràng sự khác biệt cốt lõi giữa Gateway VPC Endpoint và Interface VPC Endpoint về mặt kiến trúc, chi phí và cách thức định tuyến.
* Hiểu sâu nguyên lý chuyển hướng gói tin của Gateway VPC Endpoint dựa trên Prefix List trong Route Table mà không làm thay đổi địa chỉ IP của EC2.
* Chuẩn bị thành công môi trường VPC kiểm thử với máy chủ EC2 Private bị cô lập hoàn toàn khỏi Public Internet.
* Khởi tạo thành công Gateway VPC Endpoint cho dịch vụ Amazon S3 và tự động liên kết đường định tuyến vào Route Table của Private Subnet.
* Kiểm chứng thực tế: EC2 trong Private Subnet có thể truy vấn `aws s3 ls` và tải file lên S3 mượt mà với độ trễ thấp dù không có NAT Gateway hay Internet Gateway.
* Chứng minh tính hiệu quả tối ưu chi phí (tiết kiệm chi phí Data Processed của NAT Gateway) khi ứng dụng truyền tải lượng dữ liệu lớn với Amazon S3.
* Hoàn thành đầy đủ bộ tài liệu minh chứng và chụp ảnh các bước cấu hình thuộc chương 5.3 của Báo cáo Thực tập.
