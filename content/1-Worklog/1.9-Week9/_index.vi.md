---
title: "Worklog Tuần 9"
date: 2026-07-06
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9

* Xây dựng trang Phân tích & Báo cáo chi tiêu (Spending Analysis & Reports UI) với các biểu đồ trực quan trong Angular.
* Tích hợp các loại biểu đồ chi tiêu với thư viện **Ngx-charts / Chart.js**: Pie Chart (tỷ lệ danh mục), Bar Chart (so sánh thu chi), Line Chart (xu hướng chi tiêu).
* Thiết kế bộ lọc thời gian linh hoạt (ngày, tuần, tháng, quý, năm, khoảng tùy chỉnh) và hiển thị chỉ số tăng/giảm.
* Xây dựng giao diện trang Trợ lý AI Insight & Chatbot tài chính (AI Assistant Chat UI).
* Thiết kế giao diện trò chuyện chat hiện đại (Sidebar lịch sử hội thoại, khung chat chính, ô nhập câu hỏi).
* Thiết kế các khung tin nhắn (Message bubbles), hiệu ứng AI đang phản hồi (Typing indicator animation) và câu hỏi gợi ý mẫu.
* Xử lý các trạng thái rỗng (Empty State), lỗi kết nối (Error State) và kiểm tra responsive trên mobile.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thiết kế bố cục trang Phân tích & Báo cáo chi tiêu (Analysis Page).<br>- Tích hợp thư viện biểu đồ **Ngx-charts / Chart.js** vào Angular module xây dựng Pie Chart phân bổ tỷ lệ chi tiêu theo danh mục và Bar Chart so sánh tổng thu nhập vs tổng chi tiêu hàng tháng.<br>- Định cấu hình màu sắc tương phản cao và tooltip hiển thị số tiền khi hover. | 06/07/2026 | 06/07/2026 | [Ngx-charts Guide](https://swimlane.github.io/ngx-charts/)<br>[Data Visualization UX](https://uxdesign.cc/) |
| 3 | - Xây dựng Line Chart theo dõi biến động chi tiêu theo chuỗi thời gian.<br>- Phát triển Thanh công cụ bộ lọc thời gian (Time Filter Bar): chọn xem theo Tuần, Tháng, Quý, Năm hoặc chọn khoảng ngày tùy chỉnh.<br>- Thiết kế các Card hiển thị chỉ số xu hướng chi tiêu (% tăng/giảm so với kỳ trước kèm mũi tên chỉ hướng sinh động). | 07/07/2026 | 07/07/2026 | [Financial Chart Patterns](https://dribbble.com/) |
| 4 | - Thiết kế cấu trúc giao diện trang Trò chuyện với AI (AI Assistant Chat Component).<br>- Xây dựng Sidebar danh sách các cuộc hội thoại cũ với các chức năng: Tạo cuộc trò chuyện mới, Đổi tên, Xóa lịch sử chat.<br>- Thiết kế Khung nhắn tin chính ở trung tâm với ô nhập liệu câu hỏi tích hợp nút Gửi. | 08/07/2026 | 08/07/2026 | [Chat Interface UI Patterns](https://uicoach.io/) |
| 5 | - Thiết kế các khung tin nhắn (Message Bubbles) phân biệt rõ ràng giữa Người dùng (User align right) và Trợ lý AI (AI align left kèm avatar AI).<br>- Xây dựng hiệu ứng visual AI đang suy nghĩ / trả lời (Typing Indicator Animation với ba chấm nhấp nháy).<br>- Thiết kế danh sách các Prompt gợi ý mẫu (Prompt Chips: "Phân tích chi tiêu tháng này", "Lập kế hoạch tiết kiệm") trên màn hình bắt đầu. | 09/07/2026 | 09/07/2026 | [AI Chatbot UX Best Practices](https://uxplanet.org/) |
| 6 | - Thiết kế các trường hợp thông báo lỗi khi mất kết nối AI API hoặc hết hạn sử dụng dịch vụ.<br>- Kiểm tra khả năng hiển thị responsive của các biểu đồ báo cáo và giao diện Chat AI trên thiết bị di động.<br>- Tối ưu hóa hiệu năng render biểu đồ; Rà soát tổng thể tuần 9. | 10/07/2026 | 10/07/2026 | [Ngx-charts Responsive](https://swimlane.github.io/ngx-charts/) |

### Kết quả đạt được tuần 9

* Hoàn thành trang Phân tích chi tiêu với hệ thống biểu đồ Ngx-charts Pie Chart, Bar Chart và Line Chart vô cùng sống động.
* Phát triển bộ lọc thời gian báo cáo đa dạng đáp ứng nhu cầu xem dữ liệu chi tiết của người dùng.
* Hiển thị trực quan chỉ số so sánh xu hướng chi tiêu tăng/giảm theo từng chu kỳ tài chính.
* Hoàn thành thiết kế giao diện trang Trò chuyện với AI trong Angular theo chuẩn ứng dụng chat hiện đại.
* Tích hợp bộ danh sách Prompt Chips gợi ý câu hỏi giúp người dùng dễ dàng bắt đầu hội thoại với AI.
* Xây dựng hiệu ứng Typing animation phản hồi chân thực mang lại cảm giác tương tác tự nhiên.
* Xử lý chu đáo các màn hình trạng thái rỗng và thông báo lỗi kết nối AI.
* Đảm bảo các biểu đồ tài chính và giao diện chat AI tương thích chuẩn 100% trên màn hình điện thoại di động.
