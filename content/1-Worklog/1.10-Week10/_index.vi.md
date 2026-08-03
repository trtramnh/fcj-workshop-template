---
title: "Worklog Tuần 10"
date: 2026-07-20
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10

* Cấu hình AWS IoT Core Topic Rules để tự động lưu trữ dữ liệu thời tiết thu thập từ thiết bị biên vào Amazon S3 Raw Data Lake Bucket.
* Lập trình các hàm AWS Lambda (Python/Node.js) xử lý logic và cấu hình Amazon API Gateway cung cấp RESTful API cho ứng dụng web.
* Cấu hình AWS Glue Crawler tự động quét cơ sở dữ liệu trên S3 và tạo tác vụ Glue ETL Job chuyển đổi dữ liệu thô sang dạng bảng để phân tích.
* Triển khai ứng dụng Web Dashboard Next.js trên AWS Amplify, tích hợp Amazon Cognito User Pool để xác thực 5 tài khoản nghiên cứu viên.
* Nghiên cứu bài mẫu kiến trúc Đám mây nâng cao (Amazon ECS Fargate, ALB, Aurora Serverless v2, ElastiCache) chuẩn bị cho việc viết Blog kỹ thuật.

### Các công việc thực hiện trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Cấu hình AWS IoT Core Rule lắng nghe topic `lab/weather/telemetry`.<br>- Thiết lập Action ghi dữ liệu JSON trực tiếp vào Amazon S3 Raw Bucket theo đường dẫn phân vùng `year=YYYY/month=MM/day=DD/`.<br>- Kiểm thử gửi bản tin MQTT mô phỏng từ client và xác nhận file JSON được lưu tự động trên S3. | 20/07/2026 | 20/07/2026 | [AWS IoT S3 Action](https://docs.aws.amazon.com/iot/latest/developerguide/s3-rule-action.html)<br>[AWS IoT Rule Tutorial](https://docs.aws.amazon.com/iot/latest/developerguide/iot-write-to-s3.html) |
| Thứ 3 | - Lập trình AWS Lambda Function xử lý truy vấn dữ liệu thời tiết gần nhất.<br>- Tạo Amazon API Gateway REST API kết nối tới hàm Lambda qua phương thức GET `/api/weather/latest`.<br>- Cấu hình CORS và kiểm tra API endpoint với Postman. | 21/07/2026 | 21/07/2026 | [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)<br>[API Gateway Integration](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started-with-lambda-integrations.html) |
| Thứ 4 | - Khởi tạo AWS Glue Crawler quét định kỳ S3 Raw Bucket để tự động trích xuất schema dữ liệu vào Glue Data Catalog.<br>- Viết Glue ETL Job (PySpark) để lọc dữ liệu nhiễu và lưu sang S3 Processed Bucket dưới dạng nén Parquet giúp truy vấn nhanh hơn.<br>- Thực thi Glue Job và kiểm tra dữ liệu kết quả trên S3. | 22/07/2026 | 22/07/2026 | [AWS Glue Crawlers](https://docs.aws.amazon.com/glue/latest/dg/add-crawler.html)<br>[AWS Glue ETL Jobs](https://docs.aws.amazon.com/glue/latest/dg/author-job.html) |
| Thứ 5 | - Khởi tạo Amazon Cognito User Pool `WeatherLabUserPool` và cấu hình App Client.<br>- Đưa giao diện Web Next.js lên AWS Amplify qua Git repository.<br>- Tích hợp thư viện Amplify Auth (Cognito) vào giao diện Dashboard để bảo mật màn hình giám sát thời tiết. | 23/07/2026 | 23/07/2026 | [AWS Amplify Hosting](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html)<br>[Amplify Authentication](https://docs.amplify.aws/lib/auth/getting-started/q/platform/js/) |
| Thứ 6 | - **Nghiên cứu kiến trúc ứng dụng Web có khả năng mở rộng (Scalable Architecture):**<br>- Phân tích bài toán chịu tải cao của các hệ thống Thương mại Điện tử.<br>- Nghiên cứu sự phối hợp giữa Route 53 -> CloudFront -> AWS WAF -> ALB -> Amazon ECS Fargate -> ElastiCache -> Aurora Serverless v2.<br>- Thu thập tài liệu AWS Solutions Guidance làm cơ sở viết bài Blog kỹ thuật 1. | 24/07/2026 | 24/07/2026 | [AWS Web Store Guidance](https://docs.aws.amazon.com/solutions/web-store-on-aws/)<br>[Amazon ECS Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html) |

### Kết quả đạt được tuần 10

* Triển khai thành công AWS IoT Core Rule tự động thu thập bản tin MQTT và lưu trữ dạng phân vùng thời gian vào S3 Raw Bucket.
* Xây dựng thành công AWS Lambda Function và API Gateway endpoint phục vụ truy vấn dữ liệu thời tiết thời gian thực cho Client.
* Vận hành thành công AWS Glue Crawler và Glue ETL Job tự động trích xuất dữ liệu, làm sạch và chuyển đổi sang dạng Parquet trên S3.
* Triển khai giao diện Web Next.js lên dịch vụ AWS Amplify và hoàn thành tích hợp Amazon Cognito User Pool để xác thực người dùng.
* Nghiên cứu chuyên sâu mô hình kiến trúc Đám mây có khả năng mở rộng cao (Amazon ECS Fargate, ALB, Aurora Serverless v2, ElastiCache).
* Chuẩn bị đầy đủ sơ đồ kiến trúc và nội dung lý thuyết cho bài viết Blog kỹ thuật số 1 về Website Thương mại Điện tử.
