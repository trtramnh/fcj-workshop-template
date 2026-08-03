---
title: "Worklog Tuần 7"
date: 2026-06-22
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7

* Thiết kế lại giao diện trang Quét hóa đơn (Receipt Scan UI) hiện đại, tiện lợi.
* Xây dựng khu vực tải ảnh hóa đơn (Upload Drag & Drop Zone, chọn file, chụp ảnh từ Camera).
* Bổ sung nút chuyển nhanh sang chế độ Nhập giao dịch thủ công nếu người dùng không muốn quét ảnh.
* Thiết kế vùng Xem trước ảnh hóa đơn (Image Preview Zone) với tính năng phóng to, thu nhỏ và xoay ảnh.
* Xây dựng hiệu ứng visual trạng thái Đang xử lý OCR (Scanning radar overlay, Progress animation, Processing indicator).
* Thiết kế Form hiển thị kết quả trích xuất dữ liệu hóa đơn cho phép chỉnh sửa trước khi lưu.
* Thực hành bài lab **Workshop 5.4**: Tạo Interface VPC Endpoint (AWS PrivateLink) cho Amazon S3 hỗ trợ mô phỏng truy cập an toàn từ trung tâm dữ liệu On-premises.
* Xử lý các thông báo lỗi giao diện và cải thiện trải nghiệm trên điện thoại.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thiết kế cấu trúc bố cục trang Quét hóa đơn (Scan Receipt Page).<br>- Xây dựng Upload Drag & Drop Zone hỗ trợ kéo thả ảnh hóa đơn, chọn file từ máy tính hoặc mở Camera chụp trực tiếp trên thiết bị di động.<br>- Bổ sung nút bấm "Nhập thủ công" chuyển hướng linh hoạt. | 22/06/2026 | 22/06/2026 | [File Upload UX](https://uxdesign.cc/) |
| 3 | - Phát triển component Vùng xem trước ảnh hóa đơn (Image Preview Zone).<br>- Tích hợp các bộ công cụ điều khiển ảnh: Zoom In (+), Zoom Out (-), Rotate Left/Right và Reset View.<br>- Giúp người dùng dễ dàng soi chi tiết hình ảnh hóa đơn trước khi tiến hành quét. | 23/06/2026 | 23/06/2026 | [Image Viewer Components](https://react-image-crop.com/) |
| 4 | - Thiết kế giao diện trạng thái Đang xử lý OCR (OCR Processing Overlay).<br>- Tạo hiệu ứng Scanning Radar Animation phủ trên ảnh hóa đơn kèm Progress Bar và thông báo tiến trình sinh động.<br>- Chuẩn bị state mock giả lập phản hồi kết quả OCR từ hệ thống. | 24/06/2026 | 24/06/2026 | [CSS Scanning Animations](https://codepen.io/) |
| 5 | - **Thực hành Workshop 5 (Phần 4 - Truy cập S3 từ On-premises):**<br>&emsp; + Nghiên cứu mô hình kết nối S3 riêng tư qua AWS PrivateLink từ môi trường trung tâm dữ liệu On-premises ([Workshop S3 On-prem](5-Workshop/5.4-S3-onprem/)).<br>&emsp; + Tạo Interface VPC Endpoint cho Amazon S3, gán Subnets và Security Group.<br>&emsp; + Cấu hình Private DNS resolution và kiểm thử truy vấn hình ảnh hóa đơn lưu trên S3 qua IP nội bộ của Interface Endpoint. | 25/06/2026 | 25/06/2026 | [Workshop S3 On-prem](5-Workshop/5.4-S3-onprem/)<br>[AWS PrivateLink Interface Endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/concepts.html) |
| 6 | - Thiết kế Form Kết quả trích xuất hóa đơn (Extracted OCR Results Form) gồm Tên cửa hàng, Ngày, Tổng tiền, Danh mục gợi ý và Bảng danh sách sản phẩm.<br>- Thiết kế các thông báo lỗi và trạng thái đặc biệt (File quá 10MB, lỗi định dạng).<br>- Tối ưu hóa bố cục trang Scan trên điện thoại di động; Rà soát tuần 7. | 26/06/2026 | 26/06/2026 | [OCR Form Verification UX](https://material.io/) |

### Kết quả đạt được tuần 7

* Hoàn thành thiết kế màn hình Quét hóa đơn ấn tượng với trải nghiệm kéo thả và chụp ảnh từ camera trực quan.
* Tích hợp tùy chọn chuyển đổi nhanh sang Nhập thủ công thuận tiện.
* Xây dựng Vùng xem trước ảnh hóa đơn đầy đủ tính năng Zoom và Rotate linh hoạt.
* Tạo hiệu ứng visual Đang xử lý OCR vô cùng cuốn hút, mang lại trải nghiệm chuyên nghiệp cho người dùng.
* Thực hành thành công bài lab Workshop 5.4: Tạo và kiểm thử thành công Interface VPC Endpoint cho Amazon S3, hiểu rõ giải pháp truy cập lưu trữ S3 riêng tư cho môi trường Hybrid Cloud.
* Hoàn thành Form hiển thị kết quả trích xuất hóa đơn trơn tru, cho phép người dùng tùy chỉnh dữ liệu trước khi lưu.
* Xử lý đầy đủ các thông báo lỗi giao diện và trạng thái giới hạn file ảnh.
* Đảm bảo các thao tác chụp ảnh và kiểm tra kết quả hóa đơn mượt mà trên điện thoại di động.
