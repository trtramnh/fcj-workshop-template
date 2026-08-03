---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# SNAPTICS – AI-Powered Personal Expense Management and Receipt Scanning Platform
## Nền tảng quản lý, phân tích chi tiêu và quét hóa đơn ứng dụng trí tuệ nhân tạo trên AWS

---

### 1. Tổng quan dự án

**Snaptics** là nền tảng quản lý tài chính cá nhân và gia đình, hỗ trợ người dùng ghi nhận, theo dõi và phân tích các khoản chi tiêu một cách trực quan. Thay vì phải nhập thủ công từng giao dịch, người dùng có thể chụp hoặc tải lên hình ảnh hóa đơn. Hệ thống sử dụng công nghệ OCR và trí tuệ nhân tạo (AI) để nhận diện thông tin trên hóa đơn, bao gồm tên cửa hàng, ngày giao dịch, tổng tiền, danh mục chi tiêu và các mặt hàng liên quan.

Sau khi dữ liệu được xử lý, Snaptics tự động lưu giao dịch, phân loại chi tiêu và cập nhật vào bảng điều khiển tài chính của người dùng. Hệ thống cung cấp biểu đồ, báo cáo, ngân sách, ví dùng chung và các gợi ý tài chính dựa trên thói quen chi tiêu.

Snaptics được xây dựng theo mô hình ứng dụng web SaaS và triển khai trên nền tảng Cloud Native AWS. Kiến trúc hệ thống sử dụng các dịch vụ như **AWS Amplify**, **Amazon CloudFront**, **Amazon ECS Fargate**, **Application Load Balancer**, **Amazon SQS**, **Amazon S3**, **Amazon ECR**, **Amazon CloudWatch** và **SQL Server** theo mô hình Primary/Standby. Các tác vụ AI được tích hợp với **Azure Document Intelligence**, **Gemini API** và **OpenAI API**.

Hệ thống có hai nhóm người dùng chính:
* **User**: Quản lý giao dịch, hóa đơn, ngân sách, ví cá nhân hoặc ví gia đình, xem báo cáo và nhận gợi ý tài chính.
* **Admin**: Quản lý người dùng, thông báo, yêu cầu hỗ trợ (ticket), cấu hình hệ thống, theo dõi các tác vụ nền và giám sát hoạt động của ứng dụng.

---

### 2. Mục tiêu dự án

#### 2.1. Mục tiêu tổng quát
Xây dựng một nền tảng quản lý chi tiêu thông minh, giúp người dùng giảm thời gian nhập liệu, kiểm soát ngân sách và hiểu rõ hơn về tình hình tài chính cá nhân thông qua dữ liệu và trí tuệ nhân tạo.

#### 2.2. Mục tiêu cụ thể
* **Tự động hóa**: Tự động nhận diện thông tin từ hình ảnh hóa đơn bằng công nghệ OCR; tự động tạo và phân loại giao dịch dựa trên nội dung hóa đơn.
* **Quản lý linh hoạt**: Cho phép nhập giao dịch thủ công; quản lý nhiều ví, ngân sách cá nhân và ngân sách gia đình; hỗ trợ nhiều thành viên cùng theo dõi một ngân sách hoặc ví dùng chung.
* **Báo cáo & Phân tích**: Hiển thị báo cáo chi tiêu theo ngày, tuần, tháng và danh mục; phân tích thói quen chi tiêu và cung cấp gợi ý tài chính bằng AI; cung cấp trang trò chuyện với AI và lưu lịch sử trao đổi.
* **Cảnh báo & Thông báo**: Gửi cảnh báo khi người dùng sắp hoặc đã vượt ngân sách; xây dựng hệ thống thông báo tập trung.
* **Quản trị hệ thống**: Xây dựng trang quản trị (Admin panel) để theo dõi người dùng, yêu cầu hỗ trợ, thông báo và các tác vụ nền.
* **Vận hành & Triển khai Cloud**: Triển khai hệ thống trên AWS với khả năng mở rộng, bảo mật và giám sát tập trung; xây dựng quy trình CI/CD nhằm tự động hóa kiểm thử và triển khai.

---

### 3. Vấn đề cần giải quyết

* **Việc ghi chép chi tiêu còn thủ công**: Phần lớn người dùng vẫn ghi lại chi tiêu bằng sổ tay, Excel hoặc nhập thủ công. Quá trình này mất thời gian, dễ nhập sai và thường bị bỏ quên. Snaptics giải quyết bằng cách cho phép chụp hóa đơn và tự động trích xuất dữ liệu.
* **Dữ liệu tài chính bị phân tán**: Thông tin chi tiêu nằm ở nhiều nguồn (hóa đơn giấy, ứng dụng ngân hàng, ví điện tử). Snaptics tập trung dữ liệu giao dịch vào một hệ thống duy nhất.
* **Khó kiểm soát ngân sách**: Người dùng thường chỉ nhận ra mình đã chi quá mức sau khi ngân sách đã bị vượt. Snaptics theo dõi mức sử dụng ngân sách và gửi thông báo kịp thời.
* **Thiếu khả năng phân tích hành vi chi tiêu**: Các giao dịch riêng lẻ không cung cấp nhiều giá trị nếu không được tổng hợp. Snaptics sử dụng AI để phân tích lịch sử, phát hiện xu hướng và gợi ý điều chỉnh.
* **Khó quản lý chi tiêu gia đình**: Các thành viên khó theo dõi tổng chi tiêu chung nếu không có dữ liệu cập nhật tập trung. Snaptics cung cấp ví gia đình và ngân sách dùng chung.
* **Khả năng mở rộng của hệ thống AI**: Quá trình OCR và AI có thể gây nghẽn request. Dự án sử dụng Amazon SQS để xử lý tác vụ AI bất đồng bộ, giảm tải Backend API.

---

### 4. Kiến trúc giải pháp

Snaptics sử dụng kiến trúc cloud-native trên AWS, kết hợp ứng dụng web, container, hàng đợi xử lý bất đồng bộ, cơ sở dữ liệu dự phòng và các dịch vụ AI bên ngoài.

![Sơ đồ kiến trúc hệ thống Snaptics trên AWS Cloud](/images/2-Proposal/snaptics_architecture.jpg)

#### 4.1. Các thành phần hệ thống

* **Frontend**: Single Page Application (SPA) triển khai bằng **AWS Amplify**. Kết nối với GitHub Repository để tự động build/deploy, tích hợp **Amazon Route 53** quản lý tên miền và **Amazon CloudFront** (CDN) tối ưu phân phối nội dung qua HTTPS.
* **Backend API**: Đóng gói thành Docker Image lưu trên **Amazon ECR**, chạy trên **AWS Fargate (Amazon ECS Cluster)**. Lưu lượng truy cập được phân phối bởi **Application Load Balancer (ALB)** kết hợp **Auto Scaling** tự động điều chỉnh số lượng Fargate Task.
* **Cơ sở dữ liệu**: **SQL Server** thiết kế theo mô hình Primary/Standby nằm trong Private Subnet thuộc hai Availability Zone (Multi-AZ), quản lý dữ liệu người dùng, giao dịch, hóa đơn, ví, ngân sách, thông báo, lịch sử chat AI, ticket hỗ trợ và system audit log.
* **Lưu trữ hình ảnh**: **Amazon S3** dùng để lưu trữ ảnh hóa đơn thô, tệp giao dịch và hình ảnh đã qua xử lý, giúp tách biệt dữ liệu phương tiện khỏi cơ sở dữ liệu.
* **Xử lý OCR và AI**: 
  - Backend tải ảnh lên S3 và gửi thông điệp tới hàng đợi **Amazon SQS** (`snaptics-ai-queue`).
  - **AI Worker** trên ECS Fargate lấy message, gọi **Azure Document Intelligence** để OCR trích xuất dữ liệu, sau đó sử dụng **Gemini API** / **OpenAI API** để phân loại & phân tích giao dịch.
  - Các message lỗi nhiều lần được chuyển vào **Dead Letter Queue (DLQ)** để xử lý sự cố.
* **Hệ thống thông báo**: Quản lý tập trung trong DB kết hợp **Amazon SNS** gửi cảnh báo (vượt ngân sách, kết quả OCR, gợi ý AI, thông báo hệ thống,...).
* **Bảo mật**: Lưu secret/API Key trên **AWS Systems Manager Parameter Store**, đặt Backend/DB trong Private Subnet, áp dụng HTTPS, xác thực Access Token, phân quyền Admin/User, mã hóa dữ liệu lưu trữ và ghi nhận Audit Log.
* **CI/CD**: Tự động hóa bằng **GitHub Actions** (build Docker, push ECR, update ECS Service) và **AWS Amplify** (tự động build/deploy Frontend).
* **Giám sát & Quản lý chi phí**: **Amazon CloudWatch** (log, metric CPU/RAM, SQS Queue Depth) và **AWS Budgets** (cảnh báo chi phí vượt ngưỡng).

#### 4.2. Luồng xử lý quét hóa đơn chính
1. Người dùng tải ảnh hóa đơn từ Frontend.
2. Frontend gửi ảnh đến Backend API -> Backend lưu ảnh vào **Amazon S3**.
3. Backend đẩy message chứa thông tin ảnh vào **Amazon SQS**.
4. **AI Worker** nhận message từ SQS, gọi **Azure Document Intelligence** để trích xuất text/bảng.
5. AI Worker gửi dữ liệu đến **Gemini API / OpenAI API** để chuẩn hóa và phân loại danh mục.
6. AI Worker lưu kết quả giao dịch vào **SQL Server** và tạo thông báo cho người dùng.
7. Dashboard và Báo cáo tài chính trên Frontend tự động cập nhật.

---

### 5. Timeline dự án (12 tuần)

| Giai đoạn | Thời gian | Nội dung công việc chính | Kết quả kỳ vọng |
| :--- | :--- | :--- | :--- |
| **Giai đoạn 1** | Tuần 1–2 | Phân tích yêu cầu, xây dựng Use Case/User Flow, thiết kế Database Schema & Kiến trúc AWS | Tài liệu yêu cầu, DB Schema, Sơ đồ kiến trúc tổng thể |
| **Giai đoạn 2** | Tuần 3–5 | Phát triển Authen/Author, Quản lý giao dịch, Danh mục, Ví cá nhân/gia đình, Ngân sách & Dashboard | Hệ thống quản lý giao dịch, ví và ngân sách cơ bản |
| **Giai đoạn 3** | Tuần 6–8 | Tích hợp Azure Document Intelligence OCR, tải ảnh S3, cấu hình SQS/DLQ, Gemini/OpenAI API & AI Insight | Quy trình OCR hóa đơn tự động & gợi ý chi tiêu AI |
| **Giai đoạn 4** | Tuần 9–10 | Phát triển trang Admin (User, Ticket, Notification, System settings), tối ưu giao diện Responsive | Trang quản trị hoàn chỉnh & giao diện mobile ổn định |
| **Giai đoạn 5** | Tuần 11 | Triển khai AWS (VPC, Multi-AZ SQL Server, S3, SQS, ECS Fargate, ALB, Amplify, Route 53) & CI/CD GitHub Actions | Hệ thống vận hành hoàn chỉnh trên AWS Cloud |
| **Giai đoạn 6** | Tuần 12 | Kiểm thử chức năng, OCR/AI, bảo mật, SQS/DLQ resilience, CloudWatch monitoring & hoàn thiện tài liệu | Phiên bản sẵn sàng trình diễn (Demo-ready) |

---

### 6. Ngân sách dự kiến

#### 6.1. Môi trường phát triển và demo (Development & Demo)

| Hạng mục dịch vụ | Chi phí dự kiến / tháng |
| :--- | :--- |
| AWS Amplify, CloudFront & Route 53 | 5 – 15 USD |
| Amazon S3 | 1 – 5 USD |
| ECS Fargate (Backend & AI Worker) | 20 – 50 USD |
| Application Load Balancer (ALB) | 18 – 25 USD |
| SQL Server (Môi trường Dev) | 30 – 80 USD |
| Amazon SQS, SNS & ECR | 2 – 10 USD |
| CloudWatch & AWS Budgets | 2 – 10 USD |
| Dịch vụ AI (Azure Document Intelligence, Gemini, OpenAI) | Theo lượng request thực tế |
| **Tổng chi phí dự kiến** | **80 – 200 USD / tháng** |

#### 6.2. Môi trường Production Multi-AZ

| Hạng mục dịch vụ | Chi phí dự kiến / tháng |
| :--- | :--- |
| ECS Fargate & Application Load Balancer | 60 – 150 USD |
| SQL Server Primary/Standby (Multi-AZ) | 150 – 300 USD |
| Dual NAT Gateway & Data Transfer | 70 – 120 USD |
| S3, CloudFront, SQS, SNS, ECR & CloudWatch | 20 – 60 USD |
| Dịch vụ AI bên ngoài | Theo số hóa đơn & request |
| **Tổng chi phí dự kiến** | **300 – 600 USD / tháng** *(chưa bao gồm AI API)* |

> **Lưu ý về tối ưu chi phí:** Trong giai đoạn phát triển, dự án chạy ở mô hình Single-AZ và giới hạn runtime container để tiết kiệm. Khi lên Production, hệ thống tự động chuyển sang Multi-AZ & Auto Scaling nhằm đảm bảo tính sẵn sàng cao (High Availability).

---

### 7. Rủi ro dự án & Biện pháp giảm thiểu

| STT | Tên rủi ro | Mức độ | Biện pháp giảm thiểu & Kế hoạch dự phòng |
| :---: | :--- | :---: | :--- |
| **1** | Sai lệch nhận diện OCR (ảnh mờ, nhăn, góc nghiêng) | **Cao** | Cho phép xem lại & chỉnh sửa dữ liệu trước khi lưu; hiển thị ảnh gốc đối chiếu; cảnh báo với các trường có độ tin cậy thấp. |
| **2** | Phụ thuộc vào dịch vụ AI bên ngoài | **Cao** | Xử lý bất đồng bộ qua SQS; cơ chế Retry Exponential Backoff; đẩy task lỗi vào DLQ; thiết kế Lớp AI Service linh hoạt đổi nhà cung cấp; hỗ trợ nhập tay. |
| **3** | Rò rỉ dữ liệu tài chính cá nhân | **Rất cao** | Bắt buộc mã hóa HTTPS; lưu Secret trong SSM Parameter Store; phân quyền chi tiết Admin/User; kiểm tra Data Ownership ở Backend; Audit Logging. |
| **4** | Chi phí AWS & AI tăng ngoài dự kiến | **Trung bình - Cao** | Cài đặt AWS Budgets cảnh báo ngưỡng 50%, 80%, 100%; nén ảnh trước khi upload; thiết lập S3 Lifecycle Policy; rate-limit request AI. |
| **5** | Quá tải khi nhiều người quét hóa đơn | **Trung bình** | Sử dụng Amazon SQS lưu hàng đợi; Auto Scaling AI Worker dựa trên SQS Queue Depth; tách biệt hoàn toàn Backend API và AI Worker. |
| **6** | Lỗi khi triển khai phiên bản mới | **Trung bình** | Tự động hóa CI/CD; quản lý Env riêng biệt; triển khai Rolling Update trên ECS; Health Check tự động; lưu Docker Tag cũ để Rollback khẩn cấp. |
| **7** | Lời khuyên AI chưa sát thực tế | **Trung bình** | Chỉ dùng dữ liệu đã xác thực; hiển thị AI Insight dưới dạng gợi ý tham khảo; thu thập phản hồi người dùng để cải thiện Prompt. |

---

### 8. Kết quả kỳ vọng

* **Hoàn thiện giải pháp SaaS**: Vận hành thông suốt toàn bộ chu trình từ chụp hóa đơn, trích xuất dữ liệu, lưu trữ, đến phân tích và cảnh báo tài chính.
* **Trải nghiệm người dùng vượt trội**: Tối ưu thời gian nhập liệu thủ công nhờ OCR kết hợp bước xác nhận dữ liệu thông minh.
* **Chứng minh kiến trúc Cloud-Native**: Khẳng định tính hiệu quả của mô hình xử lý bất đồng bộ (SQS), cơ sở dữ liệu dự phòng (Multi-AZ DB), giám sát tập trung (CloudWatch) và quy trình CI/CD trên AWS.
* **Nền tảng mở rộng**: Tạo tiền đề để thu thập phản hồi người dùng thực tế, nâng cao độ chính xác AI và tiếp tục phát triển các tính năng mở rộng trong tương lai.