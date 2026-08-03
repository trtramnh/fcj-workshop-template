---
title: "Worklog Tuần 8"
date: 2026-06-29
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8

* Thiết kế giao diện Quản lý Ví (Wallet Management UI) phân biệt rõ Ví cá nhân và Ví dùng chung gia đình.
* Xây dựng Form Modal tạo mới, chỉnh sửa ví, lựa chọn icon và màu sắc đại diện cho từng ví.
* Xây dựng Modal Mời thành viên tham gia ví gia đình (nhập email, phân quyền xem/chỉnh sửa, danh sách thành viên hiện tại).
* Thiết kế giao diện Quản lý Ngân sách (Budget Management UI) với thanh tiến trình trực quan.
* Hiển thị phần trăm ngân sách đã sử dụng với cơ chế chuyển đổi màu sắc linh hoạt (Xanh < 70%, Vàng 70-90%, Đỏ > 90%).
* Thiết kế các banner cảnh báo trực quan khi chi tiêu tiệm cận hoặc vượt quá hạn mức ngân sách.
* Hỗ trợ ngân sách gia đình có nhiều thành viên và thiết kế các màn hình trạng thái Chưa có dữ liệu (Empty State).

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thiết kế bố cục trang Quản lý Ví (Wallet Management Page).<br>- Tạo tab chuyển đổi giữa Ví cá nhân (Personal Wallets) và Ví gia đình (Family Wallets).<br>- Xây dựng các Wallet Card hiển thị tên ví, số dư hiện tại, biểu tượng, danh sách thành viên tham gia và menu thao tác. | 29/06/2026 | 29/06/2026 | [Wallet Interface Design](https://dribbble.com/) |
| 3 | - Xây dựng Form Modal Tạo mới / Chỉnh sửa Ví: chọn loại ví, tên ví, số dư ban đầu, loại tiền tệ và bộ chọn biểu tượng/màu sắc.<br>- Thiết kế Modal Mời thành viên tham gia ví gia đình: ô nhập email người nhận, phân quyền (Xem / Chỉnh sửa) và danh sách thành viên đã tham gia ví. | 30/06/2026 | 30/06/2026 | [Collaborative UI Patterns](https://uxdesign.cc/) |
| 4 | - Thiết kế giao diện trang Quản lý Ngân sách (Budget Management Page).<br>- Xây dựng các Budget Card đi kèm Thanh tiến trình (Progress Bar) sinh động tự động đổi màu theo mức độ chi tiêu (Xanh: an toàn, Vàng: cảnh báo 70-90%, Đỏ: vượt ngân sách >90%).<br>- Hiển thị rõ tổng ngân sách, số tiền đã chi và số tiền còn lại. | 01/07/2026 | 01/07/2026 | [Progress Bar Indicators](https://material.io/components/progress-indicators) |
| 5 | - Thiết kế Banner cảnh báo nguy cơ vượt ngân sách hiển thị nổi bật trên Dashboard và trang Ngân sách.<br>- Xây dựng Modal Tạo ngân sách mới (chọn hạn mức, danh mục chi tiêu áp dụng, khoảng thời gian và lựa chọn cá nhân/gia đình).<br>- Hỗ trợ hiển thị thành viên đóng góp vào ngân sách gia đình. | 02/07/2026 | 02/07/2026 | [Notification Banner UX](https://uxplanet.org/) |
| 6 | - Thiết kế các component Trạng thái chưa có dữ liệu (Empty States) cho màn hình Ví và Ngân sách với hình minh họa dễ thương và nút "Tạo mới".<br>- Kiểm tra hiển thị responsive của giao diện Ví và Ngân sách trên các thiết bị di động.<br>- Tinh chỉnh khoảng cách padding/margin; Rà soát nội dung tuần 8. | 03/07/2026 | 03/07/2026 | [Empty State Design Patterns](https://emptystat.es/) |

### Kết quả đạt được tuần 8

* Hoàn thành thiết kế màn hình Quản lý Ví phân chia rõ ràng giữa Ví cá nhân và Ví dùng chung gia đình.
* Xây dựng Form Modal Tạo/Sửa Ví sinh động với khả năng cá nhân hóa màu sắc và biểu tượng.
* Hoàn thiện Modal Mời thành viên gia đình hỗ trợ nhập email và phân quyền minh bạch.
* Thiết kế màn hình Quản lý Ngân sách cực kỳ trực quan với thanh tiến trình tự động đổi màu theo % đã dùng.
* Tích hợp thành công các Banner cảnh báo vượt ngân sách thu hút sự chú ý của người dùng.
* Hỗ trợ xem ngân sách gia đình đa thành viên rõ ràng và minh bạch.
* Thiết kế các màn hình Empty State thân thiện thúc đẩy tương tác người dùng.
* Đảm bảo giao diện Ví và Ngân sách tương thích responsive mượt mà trên điện thoại di động.
