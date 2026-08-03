---
title: "Worklog Tuần 6"
date: 2026-06-15
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6

* Thiết kế giao diện Dashboard tổng quan hiển thị số dư, tổng thu, tổng chi và ngân sách khả dụng.
* Xây dựng bộ Angular Stat Card Components thống kê tài chính trực quan với biểu tượng và chỉ số biến động.
* Thiết kế màn hình Danh sách giao dịch (Transaction List UI) hỗ trợ dạng bảng trên Desktop và dạng Card trên Mobile.
* Xây dựng Form Modal / Dialog thêm mới và chỉnh sửa giao dịch với trải nghiệm người dùng tối ưu.
* Phát triển bộ lọc giao dịch nâng cao theo khoảng thời gian, loại giao dịch và danh mục.
* Thiết kế giao diện Quản lý danh mục chi tiêu (Expense Category Management UI).
* Kiểm tra hiển thị dữ liệu giả lập (Mock Data Service) ở giai đoạn đầu và tinh chỉnh giao diện Responsive.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thiết kế cấu trúc bố cục trang Dashboard tổng quan (Overview Dashboard).<br>- Xây dựng bộ Angular Stat Cards tài chính: Số dư hiện tại, Tổng thu nhập, Tổng chi tiêu và Số dư ngân sách khả dụng.<br>- Tích hợp các icon minh họa trực quan và phối màu gradient tạo ấn tượng cao cấp. | 15/06/2026 | 15/06/2026 | [Dashboard Design Best Practices](https://uxdesign.cc/) |
| 3 | - Thiết kế giao diện màn hình Danh sách giao dịch (Transaction List Page).<br>- Xây dựng component Bảng danh sách trên Desktop với các cột: Ngày, Tên giao dịch, Danh mục, Ví, Số tiền (âm/dương màu sắc phân biệt) và Thao tác.<br>- Thiết kế dạng Card view thay thế cho bảng khi hiển thị trên giao diện Mobile. | 16/06/2026 | 16/06/2026 | [Responsive Data Tables](https://css-tricks.com/responsive-data-tables/) |
| 4 | - Phát triển Form Modal / Dialog thêm mới và chỉnh sửa giao dịch (Transaction Form Dialog).<br>- Xây dựng các ô nhập liệu bằng Angular Reactive Forms: Số tiền, Ngày giao dịch, Danh mục, Ví áp dụng và Ghi chú.<br>- Thiết lập dữ liệu Mock Data Service trong Angular để kiểm thử luồng hiển thị và thao tác thêm/sửa trên UI. | 17/06/2026 | 17/06/2026 | [Angular Material Dialog](https://material.angular.io/components/dialog/overview) |
| 5 | - Xây dựng Thanh công cụ bộ lọc (Filter Bar Component): tìm kiếm theo từ khóa, lọc theo khoảng ngày, loại thu/chi và danh mục chi tiêu.<br>- Thiết kế trang Quản lý danh mục chi tiêu (Expense Category UI) dạng Grid Card cho phép xem danh mục mặc định và tạo danh mục tùy chỉnh. | 18/06/2026 | 18/06/2026 | [UI Filtering Patterns](https://uxplanet.org/) |
| 6 | - Kiểm tra giao diện Dashboard, Transaction List và Category Mgmt trên điện thoại di động.<br>- Khắc phục các lỗi tràn chữ, vỡ khung bảng giao dịch trên màn hình nhỏ.<br>- Tối ưu hóa khoảng cách padding/margin và lưu trữ kết quả thực hiện tuần 6. | 19/06/2026 | 19/06/2026 | [Mobile UI Checklist](https://material.io/design) |

### Kết quả đạt được tuần 6

* Hoàn thành thiết kế giao diện Dashboard tổng quan sắc nét với bộ Angular Stat Cards thống kê tài chính trực quan.
* Xây dựng thành công màn hình Danh sách giao dịch với khả năng tự động chuyển đổi giữa Bảng Desktop và Card Mobile.
* Hoàn thiện Form Dialog thêm mới và chỉnh sửa giao dịch với giao diện thân thiện, dễ tương tác.
* Phát triển bộ lọc giao dịch đa tiêu chí hỗ trợ tìm kiếm và phân loại dữ liệu linh hoạt trong Angular.
* Thiết kế thành công trang Quản lý danh mục chi tiêu phân biệt rõ ràng bằng màu sắc và icon.
* Kiểm thử hiển thị thành công với Angular Mock Data Service cho toàn bộ các màn hình giao dịch.
* Đảm bảo giao diện Dashboard và Transaction đáp ứng chuẩn responsive 100% trên cả Desktop và Mobile.
