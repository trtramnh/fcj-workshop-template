---
title: "Worklog Tuần 12"
date: 2026-07-27
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Mục tiêu tuần 12

* Đọc tài liệu Swagger API và trao đổi trực tiếp với thành viên Backend để xác định danh sách Endpoints, Request Body và Response Format.
* Xây dựng và cập nhật lớp Frontend API Service bằng **Angular HttpClient** và **Angular HttpInterceptor** để tự động đính kèm Bearer Access Token (JWT) và xử lý tập trung lỗi token hết hạn.
* Bỏ hoàn toàn dữ liệu giả lập (Mock Data Service) và thay thế bằng dữ liệu thật từ Backend API.
* Tích hợp thành công các API: Đăng nhập/Đăng ký, Giao dịch, Ví, Ngân sách, Quét hóa đơn, Thông báo, AI Insight, Support Ticket và Admin APIs.
* Xử lý mượt mà các trạng thái giao diện: Skeleton Loading Component, thông báo Toast báo lỗi/thành công, lỗi kết nối mạng (Network Error) và dữ liệu rỗng (Empty State).
* Tinh chỉnh Angular Reactive Form Validation và khắc phục lỗi cuộn trang bị nảy/lệch thanh Navigation bar trên thiết bị di động.
* Sửa lỗi responsive trên trang Settings và sửa lỗi menu tài khoản dropdown bị che mờ hoặc đơ đứng.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu chi tiết tài liệu Swagger UI do bộ phận Backend cung cấp.<br>- Họp thống nhất với các thành viên Backend về danh sách Endpoints, phương thức HTTP (GET, POST, PUT, DELETE), cấu trúc dữ liệu JSON và mã lỗi (Error Codes).<br>- Xây dựng Angular `AuthInterceptor` (`HTTP_INTERCEPTORS`) đính kèm JWT Bearer Token vào Request Header và xử lý tập trung lỗi 401 Unauthorized, 403 Forbidden và 500 Server Error qua RxJS `catchError`. | 27/07/2026 | 27/07/2026 | [Angular HttpClient Guide](https://angular.io/guide/http)<br>[Angular Interceptors](https://angular.io/guide/http-intercept-requests-and-responses) |
| 3 | - Loại bỏ dữ liệu mock data service và tích hợp API thật qua Angular Services cho nhóm chức năng Xác thực (Auth), Dashboard tổng quan, Danh sách giao dịch và Quản lý danh mục chi tiêu.<br>- Xây dựng hiệu ứng Skeleton Loading hiển thị khung xương trang web mượt mà trong thời gian chờ API phản hồi dữ liệu qua RxJS Observables. | 28/07/2026 | 28/07/2026 | [RxJS Observables Guide](https://rxjs.dev/guide/observable) |
| 4 | - Tích hợp API thật cho các tính năng Quét hóa đơn (gửi đường dẫn ảnh S3 và nhận kết quả trích xuất OCR), Quản lý Ví cá nhân/gia đình, Ngân sách chi tiêu, Chatbot AI Insight và Support Ticket.<br>- Tích hợp dịch vụ thông báo Toast (`Ngx-Toastr`) hiển thị thông báo tức thì khi người dùng thực hiện thao tác thành công hoặc thất bại. | 29/07/2026 | 29/07/2026 | [Ngx-Toastr Guide](https://ngx-toastr.vercel.app/) |
| 5 | - Tích hợp các API cho khu vực Admin Panel (Quản lý người dùng, Quản lý Ticket, Quản lý Thông báo hệ thống và Hangfire Jobs).<br>- Sửa lỗi responsive vỡ khung trên màn hình Cài đặt tài khoản (Settings Page) khi co nhỏ kích thước cửa sổ trình duyệt.<br>- Khắc phục lỗi Menu Dropdown tài khoản bị che khuyết hoặc không đóng khi click ra ngoài. | 30/07/2026 | 30/07/2026 | [UI Bug Fixing Techniques](https://developer.mozilla.org/) |
| 6 | - Khắc phục sự cố thanh Navigation mobile bị xô lệch/giật nảy nội dung khi vuốt cuộn trang (khóa thanh navigation bằng `position: sticky/fixed` và xử lý overflow CSS).<br>- Thực hiện kiểm thử lại Form Validation trên tất cả các trang, đảm bảo hiển thị lỗi minh bạch khi nhập sai định dạng dữ liệu.<br>- Rà soát tính nhất quán UI/UX và kiểm tra độ ổn định kết nối API toàn hệ thống. | 31/07/2026 | 31/07/2026 | [Mobile Navigation CSS Fixes](https://css-tricks.com/) |

### Kết quả đạt được tuần 12

* Nghiên cứu kỹ Swagger API và thống nhất 100% các endpoint tích hợp với đội ngũ Backend.
* Phát triển thành công Angular API Service & HttpInterceptor quản lý kết nối HTTP, tự động xử lý Access Token JWT và lỗi mạng tập trung qua RxJS.
* Loại bỏ hoàn toàn dữ liệu Mock Data và thay thế thành công bằng dữ liệu thật từ Backend API cho toàn bộ ứng dụng Snaptics.
* Giao diện phản hồi trực quan với các trạng thái Skeleton Loading, Toast Notification minh bạch và xử lý dữ liệu rỗng chu đáo.
* Khắc phục dứt điểm lỗi thanh Navigation mobile bị xô lệch/nảy khung hình khi kéo cuộn trang.
* Sửa hoàn tất các lỗi hiển thị responsive trên trang Cài đặt và Menu Dropdown tài khoản.
* Đảm bảo hệ thống kiểm tra lỗi Angular Reactive Form Validation hoạt động chính xác trên 100% màn hình nhập liệu.
* Toàn bộ hệ thống Frontend Angular đạt độ hoàn thiện cao, vận hành thông suốt với Backend API và sẵn sàng cho giai đoạn đóng gói.
