---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# AWS Đã Nâng Cấp Amazon Cognito Với Mức Gián Đoạn Gần Như Bằng 0

Authentication là một trong những thành phần quan trọng nhất của các ứng dụng hiện đại. Chỉ cần hệ thống xác thực gặp sự cố trong vài phút cũng có thể khiến người dùng không thể đăng nhập, đặt lại mật khẩu hoặc truy cập các dịch vụ cần thiết. Vì vậy, việc nâng cấp hạ tầng của một dịch vụ quản lý danh tính như Amazon Cognito đòi hỏi phải được thực hiện cẩn thận nhằm tránh ảnh hưởng đến hàng triệu người dùng.

Gần đây, AWS đã chia sẻ cách Amazon Cognito được chuyển sang hạ tầng thế hệ mới, mang đến nhiều tính năng mới trong khi vẫn duy trì khả năng tương thích ngược (backward compatibility) và giảm thiểu tối đa thời gian gián đoạn dịch vụ.

### Những tính năng mới

Hạ tầng mới của Amazon Cognito mang đến nhiều cải tiến đáng chú ý:

- **Hiệu năng cao (High-throughput Performance)**
  - Hỗ trợ hàng chục triệu người dùng trong một User Pool.
  - Xử lý hàng nghìn giao dịch mỗi giây (TPS).
  - Giảm độ trễ trong quá trình xác thực.

- **Customer-managed Encryption Keys (CMK)**
  - Tích hợp với AWS KMS.
  - Cho phép doanh nghiệp tự quản lý khóa mã hóa.
  - Tăng cường khả năng bảo mật và đáp ứng các yêu cầu về tuân thủ.

- **Multi-Region Replication**
  - Đồng bộ User Profile, Password, User Attributes và Configuration giữa nhiều AWS Region.
  - Nâng cao tính sẵn sàng và khả năng khôi phục sau sự cố của hệ thống xác thực.

### Những cải tiến trong kiến trúc

AWS đã thiết kế lại Amazon Cognito dựa trên một số nguyên tắc quan trọng:

- **Identity-first Design**
  - Tập trung tối ưu cho bài toán quản lý danh tính thay vì hoạt động như một hệ thống lưu trữ dữ liệu đa mục đích.
  - Giúp hệ thống có khả năng mở rộng và vận hành hiệu quả hơn.

- **Backward Compatibility**
  - Việc thay đổi hạ tầng không yêu cầu khách hàng phải chỉnh sửa ứng dụng hiện có.
  - Hành vi xác thực vẫn được giữ nguyên để đảm bảo tính tương thích.

- **Avoid One-way Doors**
  - Kiến trúc được thiết kế để có thể tiếp tục mở rộng và cải tiến trong tương lai mà không tạo ra những quyết định khó thay đổi.

### Chiến lược Migration

Một trong những điểm ấn tượng nhất của bài viết là cách AWS thực hiện migration cho hàng trăm triệu hồ sơ người dùng với mức gián đoạn gần như bằng không.

Quá trình migration bao gồm nhiều kỹ thuật khác nhau:

- **Shadow Mode Validation**
  - Các request được xử lý đồng thời trên cả hệ thống cũ và hệ thống mới.
  - Response, Status Code và hành vi xử lý được liên tục so sánh trước khi chuyển hoàn toàn lưu lượng sang hạ tầng mới.

- **Dual-write Architecture**
  - Dữ liệu được ghi đồng thời vào cả hai hạ tầng trong suốt quá trình migration.
  - Nếu hệ thống mới gặp sự cố, hệ thống cũ vẫn tiếp tục phục vụ người dùng.

- **Anti-entropy Validation**
  - Dữ liệu giữa hai hệ thống được đối chiếu liên tục để phát hiện sự khác biệt.
  - Hệ thống cũ đóng vai trò là nguồn dữ liệu chuẩn (Source of Truth) để đồng bộ khi cần thiết.

- **Incremental Rollout & Rollback**
  - Việc triển khai được thực hiện theo từng giai đoạn thay vì chuyển đổi toàn bộ cùng lúc.
  - Luôn duy trì khả năng rollback nếu phát sinh sự cố trong quá trình triển khai.

### Những điều mình học được

Qua bài viết này, mình nhận thấy việc hiện đại hóa hạ tầng không chỉ nhằm bổ sung các tính năng mới mà còn phải giảm thiểu rủi ro trong quá trình vận hành.

Một số bài học đáng chú ý gồm:

- Luôn kiểm chứng hệ thống mới trước khi chuyển lưu lượng thực tế.
- Thiết kế quy trình migration có khả năng rollback.
- Ưu tiên duy trì backward compatibility để tránh ảnh hưởng đến người dùng.
- Thực hiện migration theo từng giai đoạn thay vì chuyển đổi toàn bộ trong một lần.

Những kinh nghiệm này là nguồn tham khảo hữu ích khi xây dựng các ứng dụng Cloud có độ tin cậy cao cũng như triển khai các hệ thống phân tán ở quy mô lớn.

### Hình minh họa

<div style="text-align: center;">
    <img src="/fcj-workshop-template/images/3-BlogsPosted/3.1-Blog1/blog1.jpg"
         alt="Amazon Cognito Next-generation Infrastructure"
         style="width: 800px; height: auto; border-radius: 8px;">
    <p>Kiến trúc hạ tầng thế hệ mới của Amazon Cognito</p>
</div>

### Tài liệu tham khảo

Bài viết này được tổng hợp và phát triển dựa trên bài viết của **AWS Security Blog**:

- **Amazon Cognito unlocks advanced capabilities with next-generation infrastructure**

https://aws.amazon.com/blogs/security/amazon-cognito-unlocks-advanced-capabilities-with-next-generation-infrastructure/