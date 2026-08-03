---
title: "Blog 1"
date: 2026-07-27
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# KIẾN TRÚC WEBSITE THƯƠNG MẠI ĐIỆN TỬ CÓ KHẢ NĂNG MỞ RỘNG TRÊN AWS

Xin chào mọi người,

Website thương mại điện tử thường có lượng truy cập thay đổi rất lớn, đặc biệt trong các chương trình khuyến mãi hoặc mùa mua sắm cao điểm. Nếu toàn bộ request chỉ được xử lý trên một máy chủ và truy cập trực tiếp vào database, hệ thống rất dễ bị chậm, quá tải hoặc gián đoạn.

## Luồng kiến trúc tổng quát

```text
User → Route 53 → CloudFront → AWS WAF → Application Load Balancer → ECS Fargate → ElastiCache/Aurora
```

![Kiến trúc Website Thương Mại Điện Tử trên AWS](/images/3.1-Blog1/blog1.jpg)

## Cách hệ thống hoạt động

1. **Amazon Route 53**

   Định tuyến request của người dùng đến hệ thống.

2. **Amazon CloudFront**

   Phân phối nội dung từ vị trí gần người dùng, giúp giảm độ trễ và giảm tải cho hệ thống phía sau.

3. **AWS WAF**

   Kiểm tra và chặn các request có dấu hiệu bất thường trước khi chúng được chuyển đến ứng dụng.

4. **Application Load Balancer**

   Phân phối các request hợp lệ đến những container Backend đang chạy trên Amazon ECS.

5. **Amazon ECS với AWS Fargate**

   Chạy các container Backend mà không cần trực tiếp quản lý máy chủ. Hệ thống có thể tăng hoặc giảm số lượng container theo nhu cầu sử dụng.

6. **Amazon Cognito**

   Hỗ trợ đăng ký, đăng nhập và xác thực người dùng. Cognito là dịch vụ hỗ trợ xác thực và không nằm trực tiếp trên toàn bộ luồng xử lý request công khai.

7. **Amazon ElastiCache**

   Lưu tạm những dữ liệu được truy cập thường xuyên, giúp tăng tốc độ phản hồi và giảm số lần truy vấn trực tiếp đến database.

8. **Amazon Aurora Serverless v2**

   Lưu trữ dữ liệu chính của website như thông tin người dùng, sản phẩm, tồn kho và đơn hàng. Aurora Serverless v2 có thể tự động điều chỉnh tài nguyên theo khối lượng công việc.

## Giám sát và cảnh báo hệ thống

Amazon CloudWatch theo dõi hoạt động của ECS và Aurora. Khi phát hiện CPU tăng cao, ứng dụng xuất hiện nhiều lỗi hoặc database sử dụng tài nguyên bất thường, CloudWatch Alarm sẽ kích hoạt Amazon SNS để gửi cảnh báo qua email hoặc SMS.

```text
CloudWatch → CloudWatch Alarm → Amazon SNS → Email/SMS
```

## Lợi ích của kiến trúc

Nhờ kết hợp các dịch vụ trên, website có thể:

* Tăng tốc độ truy cập cho người dùng.
* Cải thiện khả năng bảo mật.
* Giảm tải cho cơ sở dữ liệu.
* Mở rộng linh hoạt khi lượng truy cập tăng cao.
* Tự động giám sát và phát hiện sự cố sớm.
* Hạn chế nguy cơ gián đoạn trong các chương trình khuyến mãi hoặc mùa mua sắm cao điểm.

## Bài viết tham khảo

* [Guidance for Web Store on AWS](https://docs.aws.amazon.com/solutions/web-store-on-aws/)
* [Guidance for Building a Containerized and Scalable Web Application on AWS](https://docs.aws.amazon.com/solutions/building-a-containerized-and-scalable-web-application-on-aws/)

#AWS #AWSArchitecture #CloudComputing #Ecommerce #ECS #Fargate #CloudFront #Aurora #CloudWatch