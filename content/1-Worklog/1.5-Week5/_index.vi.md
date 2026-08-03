---
title: "Worklog Tuần 5"
date: 2026-06-08
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5

* Khởi tạo source code dự án Frontend Single Page Application (SPA) bằng **Angular CLI** với cấu trúc dự án chuẩn mực.
* Cấu hình hệ thống **Angular Router Module** phân luồng điều hướng cho các khu vực Public, User và Admin.
* Xây dựng bộ khung Layout chính: Main Layout (Sidebar thu gọn, Top Header với User Menu, Main Content Area).
* Xây dựng hệ thống Design System trong CSS: định nghĩa bộ CSS Variables (Tokens) cho màu sắc, font chữ, khoảng cách và hiệu ứng.
* Thiết kế các Angular UI Components cơ bản dùng chung có khả năng tái sử dụng cao (Button, Input, Select, Card, Modal, Loader).
* Thiết kế giao diện Đăng nhập (Login), Đăng ký (Register) và Quên mật khẩu (Forgot Password).
* Cài đặt cơ chế bảo mật điều hướng Client (**Angular CanActivate Guards**: AuthGuard / AdminGuard) và kiểm tra responsive cơ bản.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Khởi tạo source code dự án Frontend SPA bằng Angular CLI (`ng new snaptics-frontend`) kết hợp TypeScript và SCSS/CSS Utilities.<br>- Tổ chức cấu trúc cây thư mục chuẩn: `app/core`, `app/shared`, `app/features`, `app/layouts`, `assets`, `styles`.<br>- Cài đặt các thư viện hỗ trợ giao diện: Angular Material, Lucide Angular Icons, RxJS. | 08/06/2026 | 08/06/2026 | [Angular CLI Guide](https://angular.io/cli)<br>[Angular Architecture](https://angular.io/guide/architecture) |
| 3 | - Xây dựng file CSS Design System Tokens (`styles.css` / `variables.css`).<br>- Khai báo bộ biến CSS Variables: Primary Dark Slate (`#0f172a`), Emerald Accent (`#10b981`), Neutral Colors, Spacing scale, Card Shadows, Border Radius.<br>- Xây dựng các Angular UI shared components cơ bản: `Button`, `Input`, `Select`, `Badge`. | 09/06/2026 | 09/06/2026 | [CSS Custom Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) |
| 4 | - Thiết kế giao diện trang Đăng nhập (Login) với form nhập email, mật khẩu, tùy chọn Remember Me và liên kết Quên mật khẩu.<br>- Thiết kế giao diện trang Đăng ký (Register) với các trường thông tin cá nhân và xác nhận mật khẩu.<br>- Thiết kế trang Quên mật khẩu (Forgot Password) gửi email khôi phục.<br>- Sử dụng Angular Reactive Forms để quản lý state form phía Client và validate sơ bộ. | 10/06/2026 | 10/06/2026 | [Angular Reactive Forms](https://angular.io/guide/reactive-forms) |
| 5 | - Xây dựng bộ khung Main Layout: thiết kế Sidebar thanh điều hướng thu gọn linh hoạt với logo Snaptics và danh sách menu.<br>- Thiết kế Top Header chứa thông tin tài khoản, nút thông báo và ô tìm kiếm nhanh.<br>- Cấu hình Angular Router định tuyến trang web và cài đặt cơ chế CanActivate Guards (AuthGuard & AdminGuard) ở phía Client. | 11/06/2026 | 11/06/2026 | [Angular Route Guards](https://angular.io/guide/router-tutorial-with-guards) |
| 6 | - Kiểm tra hiển thị giao diện Auth và khung Layout trên màn hình Desktop và Mobile.<br>- Tinh chỉnh các hiệu ứng chuyển đổi mượt mà (transitions, hover states).<br>- Rà soát tính khớp nối giữa thiết kế Design System và các component Angular đã dựng; Tổng kết tuần 5. | 12/06/2026 | 12/06/2026 | [Responsive Web Design](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design) |

### Kết quả đạt được tuần 5

* Khởi tạo thành công cấu trúc dự án Frontend Angular SPA sạch sẽ, mô-đun hóa cao với Feature Modules/Standalone Components.
* Định nghĩa bộ CSS Design System Tokens đồng nhất áp dụng cho toàn bộ giao diện Snaptics.
* Xây dựng bộ Angular UI Shared Components (Button, Form Input, Select Dropdown, Badge, Card Container).
* Hoàn thành thiết kế giao diện Login, Register, Forgot Password với thẩm mỹ hiện đại và Angular Reactive Forms validation.
* Xây dựng thành công bộ khung Main Layout bao gồm Sidebar thu gọn và Top Header tiện lợi.
* Định cấu hình Angular Router Module và thiết lập cơ chế CanActivate Guards (AuthGuard & AdminGuard) ở Client.
* Đảm bảo các giao diện khởi tạo hiển thị responsive mượt mà trên cả máy tính và thiết bị di động.
