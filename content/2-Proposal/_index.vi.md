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

#### 1.1. Giới thiệu
**Snaptics** là nền tảng quản lý tài chính cá nhân và gia đình được đề xuất nhằm chuyển đổi quy trình theo dõi chi tiêu từ nhập liệu thủ công sang mô hình tự động hóa, tập trung và phân tích thông minh. Hệ thống cho phép người dùng chụp hoặc tải lên hình ảnh hóa đơn; công nghệ OCR và Trí tuệ nhân tạo (AI) sẽ tự động trích xuất các thông tin trọng yếu bao gồm tên đơn vị cung cấp, ngày giao dịch, tổng giá trị, danh mục và chi tiết các mặt hàng. Sau khi người dùng kiểm tra và xác nhận, Snaptics tự động tạo giao dịch, cập nhật số dư ví, đồng bộ ngân sách và hiển thị dữ liệu trực quan trên Dashboard.

Bên cạnh quy trình quét hóa đơn tự động, Snaptics tích hợp toàn diện các tính năng: ghi nhận giao dịch thủ công, quản lý đa ví (ví cá nhân và ví gia đình dùng chung), thiết lập và cảnh báo ngân sách, báo cáo thống kê đa chiều, thông báo tập trung, phân tích hành vi chi tiêu (AI Insight) và trợ lý tương tác thông minh (AI Chatbot). Hệ thống cung cấp giao diện quản trị (Admin Panel) giúp quản trị viên theo dõi người dùng, xử lý yêu cầu hỗ trợ (ticket), phát hành thông báo, cấu hình tham số và giám sát các tác vụ nền qua Hangfire.

Snaptics được xây dựng theo mô hình ứng dụng web SaaS và triển khai trên nền tảng Cloud-Native AWS. Kiến trúc hệ thống sử dụng các dịch vụ như **AWS Amplify**, **Amazon CloudFront**, **Amazon Route 53**, **Amazon ECS Fargate**, **Application Load Balancer (ALB)**, **Amazon SQS / Dead Letter Queue (DLQ)**, **Amazon S3**, **Amazon ECR**, **AWS Systems Manager Parameter Store**, **Amazon CloudWatch**, **AWS Budgets** và **Amazon RDS for SQL Server**. Các tác vụ AI được tích hợp với **Azure Document Intelligence** và **Gemini API**.

#### 1.2. Nhóm người dùng
* **User (Người dùng cá nhân/gia đình)**: Quản lý giao dịch thu/chi, hóa đơn, ví tài chính, ngân sách và xem báo cáo thống kê; nhận cảnh báo và gợi ý tài chính thông minh từ AI; khởi tạo và tham gia chia sẻ ví hoặc ngân sách dùng chung trong gia đình.
* **Admin (Quản trị viên hệ thống)**: Quản lý người dùng, tiếp nhận và xử lý ticket hỗ trợ, phát hành thông báo hệ thống, cấu hình tham số vận hành, giám sát lịch chạy tác vụ nền qua Hangfire và theo dõi trạng thái vận hành ứng dụng.

#### 1.3. Phạm vi chức năng
* **Xác thực & Phân quyền**: Đăng nhập, xác thực người dùng (JWT/OAuth2) và phân quyền truy cập theo vai trò (User/Admin).
* **Quy trình xử lý hóa đơn**: Tải ảnh hóa đơn, trích xuất dữ liệu tự động bằng OCR, cho phép người dùng rà soát và hiệu chỉnh thông tin trước khi lưu.
* **Quản lý giao dịch**: Khởi tạo, cập nhật, phân loại tự động/thủ công và tra cứu lịch sử thu/chi.
* **Quản lý ví tài chính**: Khởi tạo ví cá nhân, ví gia đình dùng chung và phân quyền thành viên tham gia.
* **Quản lý ngân sách**: Thiết lập hạn mức chi tiêu, tự động tính toán tỷ lệ sử dụng và phát thông báo cảnh báo khi tiệm cận hoặc vượt ngưỡng.
* **Báo cáo & Thống kê**: Dashboard trực quan hiển thị biểu đồ phân tích chi tiêu theo thời gian và danh mục.
* **Tính năng AI hỗ trợ**: Phân tích hành vi chi tiêu (AI Insight), trợ lý hội thoại AI Chatbot có lưu trữ lịch sử tương tác.
* **Thông báo & Hỗ trợ**: Hệ thống thông báo tập trung trong ứng dụng và quản lý ticket hỗ trợ kỹ thuật.
* **Xử lý hệ thống & Tác vụ nền**: Lập lịch tác vụ định kỳ bằng Hangfire; xử lý trích xuất OCR/AI bất đồng bộ qua Amazon SQS và hàng đợi DLQ.
* **Vận hành Đám mây**: Triển khai, giám sát hiệu năng (Amazon CloudWatch) và quản lý chi phí (AWS Budgets) trên hạ tầng AWS.

#### 1.4. Giới hạn của giai đoạn proposal
Phạm vi thực hiện trong giai đoạn 13 tuần tập trung hoàn thiện phiên bản ứng dụng Web responsive, quy trình trích xuất hóa đơn tự động, hệ thống quản lý chi tiêu/ví/ngân sách và triển khai hạ tầng end-to-end trên AWS theo cấu hình demo tối ưu chi phí.

Các nội dung không thuộc phạm vi bắt buộc của phiên bản demo và được định hướng phát triển trong tương lai bao gồm:
* Tích hợp trực tiếp với API ngân hàng (Open Banking) hoặc các ví điện tử (MoMo, ZaloPay).
* Phát hành ứng dụng di động native (iOS / Android).
* Cung cấp các khuyến nghị tư vấn tài chính chuyên sâu mang tính cam kết pháp lý.
* Vận hành hệ thống ở quy mô Production Multi-AZ liên tục 24/7.

---

### 2. Mục tiêu dự án

#### 2.1. Mục tiêu tổng quát
Xây dựng nền tảng quản lý chi tiêu thông minh Snaptics ứng dụng công nghệ điện toán đám mây và trí tuệ nhân tạo, giúp người dùng tối ưu hóa thời gian nhập liệu, kiểm soát ngân sách chủ động và nâng cao năng lực phân tích tài chính cá nhân cũng như gia đình thông qua dữ liệu trực quan.

#### 2.2. Mục tiêu cụ thể
* **Tự động hóa**: Tự động nhận diện thông tin từ ảnh hóa đơn bằng công nghệ OCR (Azure Document Intelligence); tự động tạo và phân loại giao dịch sau khi người dùng rà soát và xác nhận.
* **Quản lý linh hoạt**: Cho phép nhập và điều chỉnh giao dịch thủ công khi không có hóa đơn; quản lý nhiều ví, ngân sách cá nhân và ngân sách gia đình; hỗ trợ nhiều thành viên cùng theo dõi dữ liệu dùng chung.
* **Báo cáo & Phân tích**: Hiển thị Dashboard và báo cáo chi tiêu theo ngày, tuần, tháng và danh mục; phân tích thói quen chi tiêu và cung cấp gợi ý tài chính (AI Insight); trợ lý hội thoại AI Chatbot có lưu lịch sử.
* **Cảnh báo & Thông báo**: Tự động theo dõi tiến độ chi tiêu, gửi cảnh báo khi ngân sách sắp đạt ngưỡng hoặc đã bị vượt; xây dựng trung tâm thông báo tập trung.
* **Quản trị hệ thống**: Xây dựng trang Admin Panel để quản lý người dùng, ticket hỗ trợ, thông báo, cấu hình hệ thống và điều phối các tác vụ nền qua Hangfire.
* **Vận hành & Triển khai Cloud**: Triển khai hệ thống trên AWS theo kiến trúc có khả năng mở rộng, bảo mật và giám sát tập trung (Amazon CloudWatch, AWS Budgets); xây dựng quy trình CI/CD tự động hóa việc build và triển khai.

---

### 3. Vấn đề cần giải quyết

* **Ghi chép chi tiêu thủ công và rủi ro sai lệch dữ liệu**: Phần lớn người dùng vẫn ghi chép bằng sổ tay, bảng tính hoặc nhập thủ công từng giao dịch. Phương pháp này gây mất thời gian, dễ phát sinh sai sót số liệu và khó duy trì đều đặn. Snaptics tự động hóa khâu thu thập dữ liệu bằng hình ảnh hóa đơn qua OCR, đồng thời cung cấp giao diện rà soát trước khi lưu trữ.
* **Dữ liệu tài chính bị phân tán trên nhiều nền tảng**: Thông tin thu chi thường nằm rải rác trên hóa đơn giấy, ứng dụng ngân hàng và ví điện tử. Snaptics đóng vai trò hợp nhất dữ liệu giao dịch về một hệ thống quản lý tập trung, giúp người dùng dễ dàng theo dõi toàn cục dòng tiền.
* **Thiếu công cụ kiểm soát và cảnh báo ngân sách theo thời gian thực**: Người dùng thường chỉ phát hiện tình trạng chi tiêu quá mức sau khi hạn mức ngân sách đã bị vượt quá. Snaptics giải quyết vấn đề này bằng cách theo dõi liên tục tiến độ chi tiêu và gửi cảnh báo kịp thời theo các ngưỡng thiết lập sẵn.
* **Hạn chế trong phân tích hành vi và xu hướng chi tiêu**: Các giao dịch đơn lẻ chưa cung cấp nhiều giá trị nếu thiếu sự tổng hợp. Snaptics phân tích dữ liệu lịch sử theo chu kỳ và danh mục, phát hiện khoản chi bất thường, nhận diện xu hướng và cung cấp gợi ý tài chính tham khảo qua AI Insight.
* **Thách thức trong quản lý và phối hợp chi tiêu gia đình**: Khi nhiều thành viên cùng đóng góp và sử dụng nguồn tài chính chung, việc thiếu kênh dữ liệu tập trung dễ dẫn đến mất kiểm soát thu chi. Ví và ngân sách gia đình dùng chung trên Snaptics giúp tất cả thành viên nắm bắt minh bạch số tiền đã chi và hạn mức còn lại.
* **Độ trễ và rủi ro tắc nghẽn khi xử lý tác vụ OCR và AI**: Nếu tác vụ OCR và AI được thực hiện hoàn toàn trong luồng request đồng bộ, hệ thống dễ rơi vào trạng thái timeout hoặc quá tải. Snaptics áp dụng Amazon SQS và AI Worker để tách biệt các tác vụ tính toán nặng khỏi luồng xử lý chính của Backend API.
* **Quản lý và lập lịch các tác vụ nền tự động**: Các tác vụ quản trị như tạo AI Insight định kỳ, kiểm tra hạn mức ngân sách và phát thông báo cần được lập lịch tự động. Trình quản lý Hangfire được tích hợp trực tiếp trong Backend .NET và cung cấp giao diện trực quan cho Admin để điều phối các tác vụ này.

---

### 4. Kiến trúc giải pháp

Snaptics được thiết kế theo kiến trúc Cloud-Native trên nền tảng AWS, kết hợp ứng dụng Web Single Page Application (SPA), hệ thống container, cơ sở dữ liệu quan hệ, lưu trữ đối tượng, hàng đợi xử lý bất đồng bộ và các dịch vụ AI chuyên dụng ngoài. Kiến trúc đảm bảo sự phân tách độc lập giữa Frontend, Backend API và AI Worker, tạo điều kiện thuận lợi cho việc triển khai, mở rộng quy mô và giám sát riêng biệt từng thành phần.

![Sơ đồ kiến trúc hệ thống Snaptics trên AWS Cloud](/images/2-Proposal/snaptics_architecture.png)

#### 4.1. Các thành phần hệ thống

* **Frontend**: Single Page Application (SPA) triển khai bằng **AWS Amplify**. Kết nối với GitHub Repository để tự động build/deploy, tích hợp **Amazon Route 53** quản lý tên miền và **Amazon CloudFront** (CDN) tối ưu phân phối nội dung qua HTTPS.
* **Backend API**: Đóng gói thành Docker Image lưu trên **Amazon ECR**, chạy trên **AWS Fargate (Amazon ECS Cluster)**. Lưu lượng truy cập được phân phối bởi **Application Load Balancer (ALB)** kết hợp **Auto Scaling** tự động điều chỉnh số lượng Fargate Task.
* **AI Worker**: Worker chạy trên ECS Fargate, nhận message từ Amazon SQS, gọi **Azure Document Intelligence** để trích xuất OCR, sau đó gọi **Gemini API** để phân loại và tạo gợi ý tài chính trước khi lưu vào cơ sở dữ liệu.
* **Cơ sở dữ liệu**: **Amazon RDS for SQL Server** đặt trong Private Subnet, quản lý dữ liệu người dùng, giao dịch, hóa đơn, ví, ngân sách, thông báo, lịch sử chat AI, ticket hỗ trợ và system audit log.
* **Lưu trữ hình ảnh**: **Amazon S3** dùng để lưu trữ ảnh hóa đơn thô, tệp giao dịch và hình ảnh đã qua xử lý, giúp tách biệt dữ liệu phương tiện khỏi cơ sở dữ liệu.
* **Hàng đợi xử lý bất đồng bộ**: **Amazon SQS** tiếp nhận tác vụ OCR/AI; **Dead Letter Queue (DLQ)** giữ các message thất bại vượt quá số lần retry để phục vụ kiểm tra lỗi.
* **Tác vụ nền & Lập lịch**: Trình quản lý **Hangfire** chạy cùng Backend .NET để lập lịch, kích hoạt và theo dõi các tác vụ định kỳ.
* **Hệ thống thông báo**: Quản lý tập trung trong DB kết hợp **Amazon SNS** gửi cảnh báo (vượt ngân sách, kết quả OCR, gợi ý AI, thông báo hệ thống).
* **Bảo mật & Cấu hình**: Lưu secret/API Key trên **AWS Systems Manager Parameter Store**, đặt Backend/Worker/DB trong Private Subnet, áp dụng HTTPS, xác thực Access Token (JWT), phân quyền Admin/User và ghi nhận Audit Log.
* **CI/CD Pipeline**: Tự động hóa bằng **GitHub Actions** (build Docker, push ECR, update ECS Service) và **AWS Amplify** (tự động build/deploy Frontend).
* **Giám sát & Quản lý chi phí**: **Amazon CloudWatch** (log, metric CPU/RAM, SQS Queue Depth) và **AWS Budgets** (cảnh báo chi phí vượt ngưỡng).

#### 4.2. Luồng xử lý quét hóa đơn chính
1. Người dùng tải ảnh hóa đơn từ Frontend.
2. Frontend gửi ảnh đến Backend API -> Backend lưu ảnh vào **Amazon S3**.
3. Backend đẩy message chứa thông tin ảnh vào **Amazon SQS**.
4. **AI Worker** nhận message từ SQS, gọi **Azure Document Intelligence** để trích xuất text/bảng.
5. AI Worker gửi dữ liệu đến **Gemini API** để chuẩn hóa và phân loại danh mục.
6. AI Worker lưu kết quả giao dịch vào **Amazon RDS for SQL Server** và tạo thông báo cho người dùng.
7. Dashboard và Báo cáo tài chính trên Frontend tự động cập nhật.

---

### 5. Timeline dự án (13 tuần)

| Giai đoạn | Thời gian | Trọng tâm & Nội dung công việc chính | Kết quả kỳ vọng |
| :--- | :--- | :--- | :--- |
| **Giai đoạn 1** | Tuần 1 | **Nghiên cứu đề tài & AWS Cloud**: Tìm hiểu yêu cầu dự án; khảo sát khó khăn trong quản lý chi tiêu; nghiên cứu mô hình SaaS và dịch vụ AWS. | Định hướng Cloud & phạm vi nghiên cứu |
| **Giai đoạn 2** | Tuần 2 | **Khảo sát yêu cầu & Thiết kế sơ bộ**: Phân tích nhu cầu quét hóa đơn, ví, ngân sách; nghiên cứu OCR/AI; lựa chọn Angular, .NET, SQL Server & AWS. | Tài liệu yêu cầu & Sơ đồ kiến trúc |
| **Giai đoạn 3** | Tuần 3 | **Hình thành ý tưởng Snaptics**: Chốt tên dự án, nhóm người dùng User/Admin, chức năng cốt lõi và phạm vi demo; xây dựng backlog. | Hoàn thiện ý tưởng & Phạm vi demo |
| **Giai đoạn 4** | Tuần 4 | **Khởi tạo mã nguồn**: Tạo GitHub Repository; khởi tạo Frontend Angular và Backend .NET; tổ chức cấu trúc mã nguồn. | Structure repository & sẵn sàng phát triển |
| **Giai đoạn 5** | Tuần 5 | **Giao dịch & Danh mục**: Phát triển API và giao diện tạo, sửa, xóa, tra cứu giao dịch; quản lý danh mục thu/chi. | Hệ thống quản lý giao dịch cơ bản |
| **Giai đoạn 6** | Tuần 6 | **Ví & Ngân sách**: Phát triển ví cá nhân, ví gia đình, thành viên dùng chung, ngân sách và logic tính mức sử dụng. | Hoàn thiện ví & Ngân sách gia đình |
| **Giai đoạn 7** | Tuần 7 | **Dashboard & Lưu trữ S3**: Xây dựng Dashboard, biểu đồ, báo cáo; phát triển giao diện quét/tải ảnh; tích hợp Amazon S3. | Dashboard trực quan & Tích hợp S3 |
| **Giai đoạn 8** | Tuần 8 | **OCR, AI & Thông báo**: Tích hợp Azure Document Intelligence; chuẩn hóa kết quả; tích hợp Gemini API; xây dựng AI Insight & Chatbot. | Trích xuất OCR & Gợi ý AI hoàn chỉnh |
| **Giai đoạn 9** | Tuần 9 | **DLQ & Hangfire**: Tạo SQS Dead Letter Queue và AI Worker bất đồng bộ; tích hợp HangfireController & Admin điều phối lịch chạy. | Tác vụ AI qua SQS/DLQ & Hangfire Admin |
| **Giai đoạn 10** | Tuần 10 | **Frontend AWS & Database**: Kết nối AWS Amplify với GitHub; đưa secret lên Parameter Store; khởi tạo RDS SQL Server demo. | Frontend live trên Amplify & RDS SQL Server |
| **Giai đoạn 11** | Tuần 11 | **VPC, SQS, ECR & Container**: Tạo VPC, SQS, Subnet mạng riêng; đóng gói Backend/Worker Docker Image; ECR & GitHub Actions. | Hạ tầng mạng & Pipeline container ECR |
| **Giai đoạn 12** | Tuần 12 | **Triển khai ECS Fargate**: Tạo ECS Cluster/Service; triển khai Backend & Worker; cấu hình ALB, Auto Scaling, CloudWatch & AWS Budgets. | Backend/Worker chạy mượt trên ECS Fargate |
| **Giai đoạn 13** | Tuần 13 | **Hoàn thiện, Kiểm thử & Demo**: Cấu hình Route 53/CloudFront; kiểm thử chức năng, responsive, phân quyền, SQS/DLQ; rà soát log & chi phí. | Phiên bản Snaptics sẵn sàng trình diễn |

---

### 6. Ngân sách dự kiến

#### 6.1. Bảng dự toán chi phí môi trường demo (1 tháng phát triển & demo)

| STT | Hạng mục dịch vụ | Cơ sở ước tính | Chi phí (USD) |
| :---: | :--- | :--- | :---: |
| **1** | AWS Amplify, CloudFront & Route 53 | Build/hosting Frontend, CDN lưu lượng thấp và 01 Hosted Zone | $4.50 |
| **2** | Amazon S3 | Lưu khoảng 20 GB ảnh hóa đơn và request upload/download | $1.00 |
| **3** | ECS Fargate - Backend & AI Worker | Task cấu hình nhỏ, tổng khoảng 200-220 giờ chạy | $8.00 |
| **4** | Application Load Balancer (ALB) | Hoạt động trong giai đoạn triển khai và demo, lưu lượng thấp | $7.00 |
| **5** | Amazon RDS for SQL Server | SQL Server Express, Single-AZ, 20 GB | $20.00 |
| **6** | NAT Gateway & Dữ liệu chuyển giao | 01 NAT Gateway, bật giới hạn trong thời gian tích hợp | $13.00 |
| **7** | Amazon SQS, SNS & ECR | Queue OCR/AI, cảnh báo cơ bản và lưu Docker Image | $1.00 |
| **8** | CloudWatch, Parameter Store & Budgets | Log, metric, alarm, secret và cảnh báo ngân sách | $3.00 |
| **9** | Azure Document Intelligence | Khoảng 1.000 trang bằng prebuilt invoice model | $10.00 |
| **10** | Gemini API | Ước tính 1 triệu token input và 200.000 token output | $0.80 |

#### 6.2. Môi trường Production Multi-AZ (Tham khảo định hướng mở rộng)

| Hạng mục dịch vụ | Chi phí dự kiến / tháng |
| :--- | :--- |
| ECS Fargate & Application Load Balancer (Auto Scaling) | $60 – $150 USD |
| SQL Server Primary/Standby (Multi-AZ) | $150 – $300 USD |
| Dual NAT Gateway & Data Transfer | $70 – $120 USD |
| S3, CloudFront, SQS, SNS, ECR & CloudWatch | $20 – $60 USD |
| External AI APIs (Azure Document Intelligence & Gemini) | Phụ thuộc số lượng hóa đơn thực tế |
| **Tổng chi phí dự kiến Production** | **$300 – $600 USD / tháng** *(chưa gồm AI API)* |

---

### 7. Rủi ro dự án & Biện pháp giảm thiểu

| STT | Rủi ro | Tác động | Khả năng | Mức độ | Biện pháp giảm thiểu & Kế hoạch dự phòng |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **1** | Sai lệch OCR do ảnh mờ, thiếu sáng hoặc hóa đơn không chuẩn | Cao | Trung bình | **Cao** | Cho phép xem ảnh gốc, rà soát và chỉnh sửa trước khi lưu; kiểm tra số tiền và đánh dấu trường có độ tin cậy thấp. |
| **2** | Azure Document Intelligence hoặc Gemini API lỗi / rate limit | Cao | Trung bình | **Cao** | Xử lý bất đồng bộ qua SQS; retry có backoff; chuyển task lỗi vào DLQ; thiết kế Lớp AI Service linh hoạt; cho phép nhập thủ công. |
| **3** | Rò rỉ dữ liệu tài chính, connection string hoặc API key | Rất cao | Thấp | **Cao** | Bắt buộc mã hóa HTTPS; lưu Secret trong SSM Parameter Store; phân quyền IAM tối thiểu; Private Subnet cho DB/Worker; Audit Logging. |
| **4** | Chi phí Cloud và AI tăng ngoài dự kiến | Cao | Trung bình | **Cao** | Thiết lập AWS Budgets cảnh báo ngưỡng 50%, 80%, 100%; giới hạn Auto Scaling; nén ảnh pre-upload; S3 Lifecycle Policy; xóa tài nguyên demo sau sử dụng. |
| **5** | Tồn đọng message khi nhiều người dùng quét cùng lúc | Trung bình | Trung bình | **Trung bình** | Theo dõi Queue Depth; tăng Worker trong giới hạn; hiển thị trạng thái xử lý; tách biệt hoàn toàn Backend API và AI Workers. |
| **6** | Phiên bản mới gây lỗi khi triển khai | Trung bình | Trung bình | **Trung bình** | Tự động hóa CI/CD kiểm thử build; ECS Health Check; lưu Docker Tag ổn định trên ECR; tách biệt môi trường Dev/Prod; CloudWatch monitoring. |
| **7** | AI Insight chung chung hoặc chưa sát thực tế | Trung bình | Trung bình | **Trung bình** | Chỉ phân tích trên dữ liệu giao dịch đã người dùng xác nhận; hiển thị AI Insight dưới dạng gợi ý tham khảo; thu thập phản hồi để tối ưu Prompt. |
| **8** | Hangfire chạy sai lịch, trùng tác vụ hoặc không hoàn thành | Trung bình | Trung bình | **Trung bình** | Kiểm tra cấu hình thời gian; chỉ Admin được điều phối; hạn chế tác vụ chạy đồng thời; ghi log chi tiết và cho phép kích hoạt lại thủ công. |
| **9** | Phạm vi chức năng vượt quá thời gian 13 tuần | Cao | Trung bình | **Cao** | Ưu tiên hoàn thiện luồng cốt lõi; khóa phạm vi demo; chia backlog bắt buộc/tùy chọn; kiểm thử sớm và lùi chức năng mở rộng sang giai đoạn sau. |

---

### 8. Kết quả kỳ vọng

* **Sản phẩm Web SaaS hoàn chỉnh**: Vận hành thông suốt toàn bộ chu trình từ chụp hóa đơn, trích xuất dữ liệu bằng OCR, quản lý ví/ngân sách cá nhân & gia đình, đến báo cáo thống kê và gửi cảnh báo tài chính.
* **Chứng minh Kiến trúc Cloud-Native trên AWS**: Thể hiện khả năng đóng gói container (ECR, ECS Fargate), xử lý hàng đợi bất đồng bộ (SQS, DLQ), lưu trữ cơ sở dữ liệu quan hệ (RDS SQL Server), bảo mật (SSM Parameter Store), giám sát tập trung (CloudWatch, Budgets) và tự động hóa CI/CD.
* **Vận hành hệ thống ổn định & Kiểm soát chi phí**: Đảm bảo hệ thống có health check, log ghi nhận đầy đủ, cơ chế xử lý lỗi qua DLQ và mô hình ngân sách tối ưu ($76.92 USD cho môi trường demo).
* **Nền tảng mở rộng linh hoạt**: Kiến trúc được thiết kế sẵn sàng để dễ dàng nâng cấp từ môi trường demo Single-AZ sang môi trường Production Multi-AZ và tích hợp các tính năng nâng cao trong tương lai (Open Banking, Mobile Native App).