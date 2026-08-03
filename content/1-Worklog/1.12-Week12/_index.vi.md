---
title: "Worklog Tuần 12"
date: 2026-08-03
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Mục tiêu tuần 12

* Kiểm tra toàn bộ mã nguồn Hugo của báo cáo thực tập FCJ Workforce, đảm bảo chuẩn xác về mốc thời gian, front matter và định dạng bảng Markdown.
* Chuẩn bị môi trường demo, kiểm thử lại toàn bộ các endpoint của dự án IoT Weather Platform và các VPC Endpoints trong bài thực hành Workshop.
* Hoàn thiện phần Tự đánh giá bản thân (`content/6-Self-evaluation/`) và tổng hợp Ý kiến phản hồi của Mentor (`content/7-Feedback/`).
* Thực thi lệnh biên dịch tĩnh Hugo (`hugo`), kiểm tra và khắc phục tất cả các cảnh báo hoặc lỗi liên kết còn tồn tại.
* Đóng gói báo cáo thực tập, thuyết trình kết quả rèn luyện trước Mentor và các thành viên FCAJ, hoàn tất chương trình thực tập 12 tuần.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Tiến hành rà soát toàn bộ cấu trúc thư mục `content/` của báo cáo thực tập Hugo.<br>- Kiểm tra tính nhất quán về ngày tháng, weight, pre-string trong front matter từ Tuần 1 đến Tuần 12.<br>- Đảm bảo tất cả các bảng công việc Markdown đều hiển thị chuẩn xác và đồng nhất giữa 2 phiên bản Tiếng Việt và Tiếng Anh. | 03/08/2026 | 03/08/2026 | [Hugo Structure Guide](https://gohugo.io/content-management/organization/)<br>[Hugo Front Matter](https://gohugo.io/content-management/front-matter/) |
| Thứ 3 | - Chuẩn bị kịch bản và môi trường demo sản phẩm phục vụ buổi báo cáo cuối kỳ.<br>- Kiểm thử hoạt động của các dịch vụ AWS trong dự án: IoT Core, Lambda, S3 Data Lake, API Gateway và Web Amplify Next.js.<br>- Re-test kết nối riêng tư S3 qua Gateway VPC Endpoint và Interface VPC Endpoint theo báo cáo Workshop 5. | 04/08/2026 | 04/08/2026 | [Proposal Document](2-Proposal/)<br>[Workshop Document](5-Workshop/) |
| Thứ 4 | - Hoàn thiện nội dung phần Báo cáo Tự đánh giá bản thân tại thư mục `content/6-Self-evaluation/`.<br>- Tổng hợp các đánh giá chuyên môn, góp ý và điểm số từ phía Mentor cùng đơn vị thực tập tại thư mục `content/7-Feedback/`.<br>- Cập nhật đầy đủ bản Tiếng Việt và Tiếng Anh cho cả 2 mục đánh giá. | 05/08/2026 | 05/08/2026 | [Self Evaluation](6-Self-evaluation/)<br>[Mentor Feedback](7-Feedback/) |
| Thứ 5 | - **Thực thi lệnh kiểm thử biên dịch website:**<br>- Chạy lệnh `hugo` trong terminal để build tĩnh trang web báo cáo thực tập.<br>- Xác nhận quá trình build diễn ra thành công, kiểm tra các tệp tin xuất ra trong thư mục `public/`.<br>- Kiểm tra lại tính chính xác của các đường dẫn tĩnh (static assets) và hình ảnh minh họa. | 06/08/2026 | 06/08/2026 | [Hugo CLI Documentation](https://gohugo.io/commands/hugo/)<br>[Hugo Build & Deployment](https://gohugo.io/hosting-and-deployment/) |
| Thứ 6 | - **Báo cáo tổng kết & Hoàn tất thực tập:**<br>- Tham gia buổi báo cáo tổng kết kết quả thực tập FCJ Workforce trước Mentor và hội đồng đánh giá.<br>- Trình bày các kiến thức đã tích lũy, các sản phẩm thực hành và bài viết blog kỹ thuật trong 12 tuần.<br>- Tiếp thu các ý kiến đóng góp cuối cùng, hoàn thiện hồ sơ thực tập và chính thức khép lại chương trình. | 07/08/2026 | 07/08/2026 | [FCJ Workforce Regulations](https://hcm-rules.awsfcaj.com/1-regulations/) |

### Kết quả dự kiến tuần 12

* Hoàn thành rà soát toàn bộ source code trang web Hugo, đảm bảo tuân thủ 100% các quy định về mốc thời gian, cấu trúc front matter và chuẩn định dạng Markdown.
* Đảm bảo sự tương đồng hoàn toàn giữa bản Tiếng Việt (`_index.vi.md`) và bản Tiếng Anh (`_index.md`) cho tất cả các tuần Worklog từ Tuần 1 đến Tuần 12.
* Chuẩn bị sẵn sàng môi trường demo trực quan cho dự án "IoT Weather Platform for Lab Research" và bài tập thực hành Workshop VPC Endpoints.
* Hoàn thiện nội dung Tự đánh giá bản thân (Self-evaluation) và ghi nhận đầy đủ nhận xét tích cực từ Mentor (Feedback).
* Biên dịch trang web báo cáo thành công với lệnh `hugo`, không phát sinh lỗi cú pháp hay hỏng đường dẫn tĩnh.
* Thuyết trình báo cáo tổng kết thực tập tự tin, chuyên nghiệp và bảo vệ thành công kết quả rèn luyện 12 tuần tại chương trình FCJ Workforce.
