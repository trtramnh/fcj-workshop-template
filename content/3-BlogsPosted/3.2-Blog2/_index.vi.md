---
title: "Blog 2"
date: 2026-07-27
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# HÀNH TRANG AWS & TƯ DUY CLOUD-NATIVE TỪ KỲ THỰC TẬP THỰC TẾ

Xin chào mọi người,

Trong quá trình thực tập và tham gia phát triển dự án thực tế tại công ty, dù thành phần Cloud chính mà mình tương tác là Amazon S3, nhưng quá trình triển khai đã giúp mình tiếp thu được rất nhiều kiến thức nền tảng quan trọng về hệ sinh thái Amazon Web Services (AWS) và tư duy Cloud-Native.

![Sơ đồ Kiến trúc Chuyển đổi Lưu trữ Đơn khối sang S3 Decoupled](/images/3.2-Blog2/architecture-diagram.jpg)

## Nội dung tiếp thu và áp dụng thực tế

1. **Hiểu và ứng dụng tư duy phân tách hệ thống (Decoupling)**

   * **Kiến thức tiếp thu:** Trước đây, mình thường có thói quen lưu trữ file tĩnh (hình ảnh, tài liệu) cục bộ ngay trên máy chủ ứng dụng (Local storage) hoặc lưu dưới dạng nhị phân trong cơ sở dữ liệu. Nhờ làm việc với AWS, mình đã hiểu được tư duy "Decoupling" - tách bạch hoàn toàn phần Xử lý tính toán (Compute) và phần Lưu trữ (Storage).
   * **Áp dụng thực tế:** Mình đã thiết kế Backend .NET chỉ đảm nhận việc xử lý logic nghiệp vụ, sau đó đẩy toàn bộ các tệp tin/chứng từ lên Amazon S3 (Object Storage) và chỉ lưu trữ đường dẫn (URL) vào Database. Điều này giúp máy chủ giảm tải hoàn toàn, không lo đầy ổ cứng và Database hoạt động nhẹ nhàng, truy xuất nhanh hơn.

2. **Làm chủ thư viện AWSSDK và thao tác thực chiến với S3**

   * **Kiến thức tiếp thu:** Chuyển đổi từ việc thao tác thủ công trên giao diện Web Console của AWS sang việc tương tác và quản lý tài nguyên hoàn toàn bằng mã nguồn (thông qua SDK).
   * **Áp dụng thực tế:** Mình đã tích hợp thành công gói NuGet AWSSDK.S3 vào hệ thống lõi. Tự tay xây dựng các class Service trong C# để xử lý trọn vẹn vòng đời của một file: từ việc stream dữ liệu tải lên (PutObjectRequest), thiết lập Metadata/ContentType chuẩn xác cho các định dạng file khác nhau, đến việc truy xuất và xóa file an toàn trên đám mây.
   * **Tư duy tối ưu chi phí (Cost Optimization):** Bên cạnh việc lưu trữ, mình cũng nhận ra kiểm soát "file rác" trên S3 quan trọng không kém. Có những trường hợp user upload file rồi hủy thao tác giữa chừng (ví dụ đóng tab, mất kết nối) khiến file mồ côi vẫn tồn tại trên Bucket dù không còn được tham chiếu trong Database. Mình đã học cách kết hợp thêm logic dọn dẹp - gọi `DeleteObjectAsync` ngay khi phát hiện transaction thất bại, hoặc dùng Lifecycle Policy của S3 để tự động xóa các file trong thư mục tạm (`temp/`) sau một khoảng thời gian nhất định. Đây là bài học thực tế về tư duy quản lý chi phí Cloud - dùng bao nhiêu trả bấy nhiêu, nhưng phải chủ động dọn dẹp để tránh lãng phí dung lượng lưu trữ vô hình chung.

3. **Quản trị quyền truy cập với AWS IAM (Identity and Access Management)**

   * **Kiến thức tiếp thu:** Nắm vững nguyên tắc cốt lõi trong bảo mật Cloud: Nguyên tắc Đặc quyền tối thiểu (Least Privilege). Mình hiểu được mức độ nguy hiểm nếu sử dụng tài khoản Root của AWS để kết nối ứng dụng.
   * **Áp dụng thực tế:** Mình đã tự thiết lập các tài khoản IAM User chuyên biệt cho môi trường phát triển. Thay vì cấp quyền Admin, mình học cách viết các IAM Policy bằng JSON để giới hạn quyền hạn: ứng dụng Backend chỉ được phép Đọc (`s3:GetObject`) và Ghi (`s3:PutObject`) vào duy nhất một Bucket cụ thể của dự án, triệt tiêu rủi ro bị phá hoại hoặc lộ lọt dữ liệu chéo.

4. **Bảo mật Secret Key & Biến môi trường**

   * **Kiến thức tiếp thu:** Nhận thức rõ ràng về các rủi ro bảo mật (Security Vulnerabilities) khi làm việc với Cloud, đặc biệt là lỗi "Hardcode" thông tin xác thực.
   * **Áp dụng thực tế:** Mình đã thiết lập quy trình quản lý thông tin nhạy cảm (Access Key, Secret Key) thông qua file cấu hình `appsettings.json` (được loại trừ khỏi Git) và sử dụng Environment Variables. Điều này đảm bảo an toàn tuyệt đối khi đẩy mã nguồn lên các kho lưu trữ như GitHub.

5. **Xử lý bài toán chia sẻ dữ liệu an toàn với Pre-signed URL**

   * **Kiến thức tiếp thu:** Hiểu được cơ chế bảo mật tài nguyên tĩnh trên Internet. Mặc định S3 khóa toàn bộ quyền truy cập công khai (Block Public Access) để bảo vệ dữ liệu nội bộ.
   * **Áp dụng thực tế:** Mình đã nghiên cứu và triển khai thành công kỹ thuật Pre-signed URL. Khi Client cần hiển thị tài liệu hoặc hình ảnh, Backend sẽ dùng thông tin xác thực của AWS để sinh ra một đường link tạm thời (chỉ có hiệu lực trong khoảng thời gian ngắn, ví dụ 15 - 30 phút). Kỹ thuật này giúp hệ thống chia sẻ file cho đúng người dùng một cách an toàn mà không cần mở toang quyền Public cho toàn bộ Bucket.

![Mã nguồn C# S3Service triển khai GeneratePreSignedUrl](/images/3.2-Blog2/s3service-code.jpg)

## Kỷ niệm thực chiến với CORS

Một bẫy thực tế mình gặp phải là khi Frontend gọi Pre-signed URL để tải file trực tiếp lên S3 thì liên tục bị trình duyệt chặn lỗi CORS (Cross-Origin Resource Sharing). Dù test bằng Postman thành công, nhưng lên trình duyệt thì thất bại do chính sách Same-Origin Policy.

Giải pháp của mình là quay lại cấu hình CORS Policy ngay trên S3 Bucket, khai báo rõ các domain được phép (`localhost` khi dev, domain staging khi test) và cho phép các method GET/PUT tương ứng. Qua sự cố này, mình hiểu rõ hơn rằng bảo mật Cloud không chỉ dừng ở tầng Server-to-Server (IAM, Secret Key) mà còn phải tính đến tầng trình duyệt (Browser-level security).

## Tổng kết

Kỳ thực tập không chỉ giúp mình biết cách sử dụng một công cụ lưu trữ (Amazon S3), mà quan trọng hơn là định hình lại tư duy thiết kế phần mềm. Những bài học về bảo mật (IAM), tối ưu hóa kiến trúc và thao tác với Cloud SDK là hành trang vững chắc để mình tiếp tục phát triển theo định hướng Kỹ sư Backend / Cloud-Native trong tương lai.

## Bài viết tham khảo

* [Amazon S3 Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
* [AWS SDK for .NET Documentation](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/welcome.html)

#AWS #AmazonS3 #CloudNative #Decoupling #AWSSDK #IAM #PreSignedURL #Backend