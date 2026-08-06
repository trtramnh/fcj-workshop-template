---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# NHỮNG BÀI HỌC VỀ AWS VÀ TƯ DUY CLOUD-NATIVE TỪ KỲ THỰC TẬP

Trong quá trình thực tập, mình có cơ hội tham gia vào một dự án Backend thực tế, trong đó Amazon S3 được sử dụng để lưu trữ tài liệu và hình ảnh. Mặc dù Amazon S3 là dịch vụ AWS chính được sử dụng trong dự án, quá trình triển khai đã giúp mình hiểu rõ hơn về cách xây dựng ứng dụng theo mô hình Cloud-Native cũng như mở rộng kiến thức về hệ sinh thái AWS.

Bên cạnh việc tìm hiểu cách tích hợp dịch vụ lưu trữ đám mây vào hệ thống Backend, mình còn học được nhiều kiến thức về thiết kế hệ thống, bảo mật, tối ưu chi phí và các thực hành tốt trên nền tảng Cloud thường được áp dụng trong các dự án thực tế.

### Những kiến thức nổi bật

Trong quá trình tham gia dự án, mình đã học được nhiều khái niệm quan trọng về Cloud-Native.

- **Tách biệt Compute và Storage**
  - Hiểu cách Backend chỉ tập trung xử lý nghiệp vụ, trong khi các tệp được lưu trữ trên Amazon S3.
  - Hiểu lý do chỉ lưu URL của đối tượng trong cơ sở dữ liệu thay vì lưu trực tiếp dữ liệu nhị phân.
  - Nhận thấy cách thiết kế này giúp hệ thống dễ mở rộng và giảm tải cho máy chủ ứng dụng.

- **Làm việc với AWS SDK for Amazon S3**
  - Tìm hiểu cách tích hợp thư viện AWSSDK.S3 vào ứng dụng .NET.
  - Tìm hiểu các thao tác như tải lên, tải xuống, quản lý metadata và xóa đối tượng bằng C#.
  - Hiểu rõ hơn cách tương tác với các dịch vụ AWS thông qua lập trình thay vì chỉ thao tác trên AWS Management Console.

- **Tối ưu chi phí**
  - Hiểu rằng các đối tượng không còn sử dụng vẫn có thể làm tăng chi phí lưu trữ trên Cloud.
  - Nhận thức được tầm quan trọng của việc dọn dẹp các tệp tạm thời.
  - Tìm hiểu Amazon S3 Lifecycle Policies để tự động xóa các đối tượng không còn cần thiết.

- **IAM và nguyên tắc phân quyền tối thiểu**
  - Hiểu lý do không nên sử dụng AWS Root Account cho ứng dụng.
  - Tìm hiểu cách IAM Users và IAM Policies được sử dụng để quản lý quyền truy cập.
  - Nhận thức được tầm quan trọng của nguyên tắc Least Privilege trong việc tăng cường bảo mật.

- **Bảo vệ thông tin xác thực AWS**
  - Hiểu lý do Access Key và Secret Key cần được lưu trữ an toàn bằng biến môi trường hoặc các tệp cấu hình không được đưa lên hệ thống quản lý mã nguồn.
  - Nhận thức được rủi ro khi hardcode thông tin xác thực trong mã nguồn.

- **Chia sẻ tệp an toàn bằng Pre-signed URL**
  - Tìm hiểu cách Amazon S3 Pre-signed URL cho phép truy cập tạm thời vào các đối tượng riêng tư.
  - Hiểu cách duy trì S3 Bucket ở chế độ riêng tư nhưng vẫn cho phép người dùng được cấp quyền truy cập.

- **Hiểu về bảo mật trình duyệt**
  - Tìm hiểu về Cross-Origin Resource Sharing (CORS) khi ứng dụng Frontend giao tiếp với Amazon S3.
  - Hiểu cách cấu hình S3 CORS để cho phép các yêu cầu hợp lệ đồng thời vẫn đảm bảo tính bảo mật.
  - Nhận thấy rằng bảo mật Cloud không chỉ nằm ở Backend mà còn liên quan đến các chính sách bảo mật của trình duyệt.

### Những điều mình học được

Kỳ thực tập giúp mình nhận ra rằng Cloud Computing không chỉ đơn thuần là sử dụng các dịch vụ AWS mà còn là quá trình thiết kế hệ thống sao cho bảo mật, dễ mở rộng, tối ưu chi phí và có khả năng vận hành ổn định.

Thông qua việc tham gia dự án thực tế, mình hiểu rõ hơn cách các dịch vụ AWS phối hợp với nhau trong một ứng dụng Cloud-Native. Những kiến thức và trải nghiệm này đã giúp mình xây dựng nền tảng vững chắc hơn về phát triển Backend trên Cloud, đồng thời tạo tiền đề cho định hướng trở thành Backend Developer và Cloud Engineer trong tương lai.

### Hình minh họa

<div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 24px; margin-top: 20px;">

  <div style="width: 420px; text-align: center;">
    <img src="/fcj-workshop-template/images/3-BlogsPosted/3.3-Blog3/blog3.jpg"
         alt="Amazon S3 Architecture"
         style="width:100%; height:260px; object-fit:contain; background:#fafafa; border-radius:10px; box-shadow:0 2px 8px rgba(0,0,0,0.15);">
    <p>Kiến trúc Cloud-Native tích hợp Amazon S3 vào ứng dụng Backend.</p>
  </div>

  <div style="width: 420px; text-align: center;">
    <img src="/fcj-workshop-template/images/3-BlogsPosted/3.3-Blog3/blog3.1.jpg"
         alt="Amazon S3 Pre-signed URL"
         style="width:100%; height:260px; object-fit:contain; background:#fafafa; border-radius:10px; box-shadow:0 2px 8px rgba(0,0,0,0.15);">
    <p>Triển khai tạo Amazon S3 Pre-signed URL bằng AWS SDK for .NET.</p>
  </div>

</div>

### Tài liệu tham khảo

Những kiến thức được tổng hợp trong bài viết này được học hỏi từ:
* [Amazon S3 Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
* [AWS SDK for .NET Documentation](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/welcome.html)