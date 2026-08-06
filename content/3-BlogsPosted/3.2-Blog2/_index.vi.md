---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# KIẾN TRÚC WEBSITE THƯƠNG MẠI ĐIỆN TỬ CÓ KHẢ NĂNG MỞ RỘNG TRÊN AWS

Các website thương mại điện tử thường có lượng truy cập thay đổi rất lớn, đặc biệt trong các chương trình khuyến mãi, Flash Sale hoặc mùa mua sắm cao điểm. Nếu toàn bộ yêu cầu đều được xử lý trên một máy chủ và truy cập trực tiếp vào cơ sở dữ liệu, hệ thống rất dễ gặp tình trạng quá tải, làm giảm hiệu năng hoặc gây gián đoạn dịch vụ.

AWS cung cấp nhiều dịch vụ được quản lý giúp xây dựng các ứng dụng có khả năng mở rộng, bảo mật và tính sẵn sàng cao. Bằng cách kết hợp các dịch vụ về mạng, bảo mật, điện toán, bộ nhớ đệm, cơ sở dữ liệu và giám sát, các hệ thống thương mại điện tử có thể đáp ứng lượng truy cập lớn mà vẫn duy trì trải nghiệm ổn định cho người dùng.

### Tổng quan kiến trúc

Luồng xử lý của hệ thống được xây dựng như sau:

**Người dùng → Amazon Route 53 → Amazon CloudFront → AWS WAF → Application Load Balancer → Amazon ECS (AWS Fargate) → Amazon ElastiCache / Amazon Aurora Serverless v2**

Mỗi dịch vụ AWS đảm nhận một vai trò riêng trong kiến trúc:

- **Amazon Route 53**
  - Phân giải tên miền và định tuyến yêu cầu của người dùng đến các tài nguyên AWS phù hợp.

- **Amazon CloudFront**
  - Phân phối nội dung từ các Edge Location gần người dùng nhằm giảm độ trễ và tăng tốc độ truy cập.

- **AWS WAF**
  - Bảo vệ ứng dụng trước các cuộc tấn công web phổ biến như SQL Injection và Cross-Site Scripting (XSS).

- **Application Load Balancer**
  - Phân phối lưu lượng truy cập đến nhiều container backend nhằm tăng khả năng mở rộng và tính sẵn sàng của hệ thống.

- **Amazon ECS với AWS Fargate**
  - Triển khai và vận hành các dịch vụ backend dưới dạng container mà không cần quản lý máy chủ.

- **Amazon Cognito**
  - Hỗ trợ đăng ký, xác thực và quản lý người dùng một cách an toàn.

- **Amazon ElastiCache**
  - Lưu trữ tạm thời các dữ liệu được truy cập thường xuyên nhằm giảm tải cho cơ sở dữ liệu và cải thiện hiệu năng.

- **Amazon Aurora Serverless v2**
  - Lưu trữ dữ liệu chính của hệ thống và tự động mở rộng tài nguyên theo khối lượng công việc.

### Giám sát và cảnh báo

Giám sát hệ thống đóng vai trò quan trọng trong việc đảm bảo ứng dụng hoạt động ổn định.

Amazon CloudWatch liên tục thu thập các chỉ số và nhật ký hoạt động từ Amazon ECS và Amazon Aurora. Khi phát hiện những dấu hiệu bất thường như CPU sử dụng cao, ứng dụng phát sinh nhiều lỗi hoặc hiệu năng cơ sở dữ liệu suy giảm, **CloudWatch Alarm** sẽ tự động kích hoạt **Amazon SNS** để gửi cảnh báo qua Email hoặc SMS.

**Luồng giám sát**

**Amazon CloudWatch → CloudWatch Alarm → Amazon SNS → Email / SMS**

### Lợi ích của kiến trúc

Kiến trúc này mang lại nhiều lợi ích:

- Cải thiện hiệu năng nhờ Amazon CloudFront và Amazon ElastiCache.
- Tăng cường bảo mật với AWS WAF và Amazon Cognito.
- Khả năng mở rộng linh hoạt nhờ Amazon ECS, AWS Fargate và Aurora Serverless v2.
- Đảm bảo tính sẵn sàng cao thông qua Application Load Balancer.
- Giám sát liên tục và cảnh báo sớm bằng Amazon CloudWatch và Amazon SNS.

### Những điều học được

Kiến trúc tham khảo này cho thấy cách các dịch vụ được quản lý trên AWS có thể phối hợp với nhau để xây dựng một website thương mại điện tử theo mô hình Cloud-Native.

Thông qua việc tìm hiểu kiến trúc này, mình hiểu rõ hơn vai trò của từng dịch vụ AWS cũng như cách các thành phần về mạng, bảo mật, container, bộ nhớ đệm, cơ sở dữ liệu và giám sát được kết hợp để xây dựng một hệ thống có khả năng mở rộng, bảo mật và sẵn sàng đáp ứng các yêu cầu trong môi trường thực tế.

### Hình minh họa

<div style="text-align: center;">
    <img src="/fcj-workshop-template/images/3-BlogsPosted/3.2-Blog2/blog2.jpg"
         alt="Kiến trúc website thương mại điện tử"
         style="width: 900px; height: auto; border-radius: 8px;">
    <p>Kiến trúc website thương mại điện tử có khả năng mở rộng trên AWS.</p>
</div>

### Tài liệu tham khảo

Bài viết được tổng hợp dựa trên các tài liệu chính thức của AWS:

- **Guidance for Web Store on AWS**
  https://docs.aws.amazon.com/solutions/web-store-on-aws/

- **Guidance for Building a Containerized and Scalable Web Application on AWS**
  https://docs.aws.amazon.com/solutions/building-a-containerized-and-scalable-web-application-on-aws/