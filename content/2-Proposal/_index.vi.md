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
Snaptics được đề xuất nhằm chuyển đổi việc quản lý chi tiêu từ nhập liệu thủ công sang quy trình số hóa, tập trung và có khả năng phân tích thông minh. Người dùng có thể chụp hoặc tải ảnh hóa đơn; hệ thống trích xuất các trường như tên cửa hàng, ngày giao dịch, tổng tiền, danh mục và các mặt hàng liên quan. Sau bước xác nhận, Snaptics tạo giao dịch, cập nhật ví và ngân sách, đồng thời đồng bộ dữ liệu lên Dashboard.

Ngoài chức năng quét hóa đơn, hệ thống hỗ trợ nhập giao dịch thủ công, quản lý nhiều ví, ngân sách cá nhân hoặc gia đình, báo cáo chi tiêu, thông báo tập trung, AI Insight và trò chuyện với AI. Trang quản trị giúp Admin theo dõi người dùng, ticket hỗ trợ, thông báo, cấu hình hệ thống và các tác vụ nền Hangfire.

#### 1.2. Nhóm người dùng
* **User (Người dùng cá nhân/gia đình)**: Quản lý giao dịch, hóa đơn, ví, ngân sách, báo cáo; nhận thông báo và gợi ý tài chính; tham gia ví hoặc ngân sách dùng chung.
* **Admin (Quản trị viên hệ thống)**: Quản lý người dùng, ticket hỗ trợ, thông báo, cấu hình hệ thống, lịch chạy Hangfire và hoạt động vận hành cần thiết.

#### 1.3. Phạm vi chức năng
* Đăng nhập, xác thực và phân quyền truy cập giữa User và Admin.
* Quét hoặc tải ảnh hóa đơn, xem lại kết quả OCR và chỉnh sửa trước khi lưu.
* Tạo, cập nhật, phân loại và tra cứu giao dịch thu/chi.
* Quản lý ví cá nhân, ví gia đình và thành viên tham gia.
* Thiết lập ngân sách, theo dõi mức sử dụng và cảnh báo vượt ngân sách.
* Dashboard, biểu đồ và báo cáo theo thời gian hoặc danh mục.
* AI Insight, trang trò chuyện AI và lưu lịch sử trao đổi.
* Hệ thống thông báo trong ứng dụng và ticket hỗ trợ.
* Tác vụ nền định kỳ bằng Hangfire; xử lý OCR/AI bất đồng bộ qua SQS.
* Triển khai, giám sát và kiểm soát chi phí trên AWS.

#### 1.4. Giới hạn của giai đoạn proposal
Giai đoạn 13 tuần tập trung vào phiên bản Web, quy trình quét hóa đơn, quản lý chi tiêu và triển khai end-to-end trên AWS. Việc kết nối trực tiếp với ngân hàng hoặc ví điện tử, phát hành ứng dụng di động native, tư vấn tài chính mang tính cam kết và vận hành production quy mô lớn không thuộc phạm vi bắt buộc của phiên bản demo.

---

### 2. Mục tiêu dự án

#### 2.1. Mục tiêu tổng quát
Xây dựng một nền tảng quản lý chi tiêu thông minh giúp người dùng giảm thời gian nhập dữ liệu, kiểm soát ngân sách và hiểu rõ hơn về tình hình tài chính cá nhân hoặc gia đình thông qua dữ liệu, tự động hóa và trí tuệ nhân tạo.

#### 2.2. Mục tiêu cụ thể
* Tự động nhận diện thông tin từ ảnh hóa đơn bằng công nghệ OCR.
* Tự động tạo và phân loại giao dịch dựa trên dữ liệu hóa đơn đã được người dùng xác nhận.
* Cho phép nhập và điều chỉnh giao dịch thủ công khi không có hóa đơn hoặc kết quả OCR chưa chính xác.
* Quản lý nhiều ví, ngân sách cá nhân và ngân sách gia đình; hỗ trợ nhiều thành viên cùng theo dõi.
* Cung cấp Dashboard và báo cáo chi tiêu theo ngày, tuần, tháng và danh mục.
* Phân tích lịch sử giao dịch và đưa ra AI Insight ở dạng gợi ý tham khảo.
* Gửi cảnh báo khi ngân sách sắp đạt ngưỡng hoặc đã bị vượt.
* Xây dựng hệ thống thông báo và trang trò chuyện AI có lưu lịch sử.
* Xây dựng trang Admin cho người dùng, ticket hỗ trợ, thông báo, cấu hình và tác vụ nền.
* Triển khai hệ thống trên AWS theo kiến trúc có khả năng mở rộng, giám sát và kiểm soát chi phí.
* Xây dựng CI/CD để tự động hóa việc build và triển khai phiên bản mới.

#### 2.3. Tiêu chí hoàn thành

| Nhóm tiêu chí | Kết quả cần đạt |
| :--- | :--- |
| **Luồng nghiệp vụ** | Thực hiện được quy trình từ tải hóa đơn đến tạo giao dịch và cập nhật Dashboard. |
| **Quản lý tài chính** | Có giao dịch, ví, ngân sách, báo cáo, thông báo và AI Insight hoạt động theo phạm vi demo. |
| **Phân quyền** | User chỉ truy cập dữ liệu của mình; Admin truy cập chức năng quản trị theo quyền được cấp. |
| **Triển khai Cloud** | Frontend, CloudFront/WAF, Backend, Worker, Database, Storage, Queue, Secrets và Monitoring được cấu hình trên AWS. |
| **Vận hành** | Có log, health check, cảnh báo chi phí và cơ chế xử lý message lỗi qua DLQ. |

---

### 3. Vấn đề cần giải quyết

#### 3.1. Việc ghi chép chi tiêu còn thủ công
Người dùng thường ghi lại chi tiêu bằng sổ tay, bảng tính hoặc nhập từng giao dịch vào ứng dụng. Cách làm này mất thời gian, dễ sai số tiền và khó duy trì đều đặn. Snaptics sử dụng ảnh hóa đơn để tự động hóa bước thu thập dữ liệu, đồng thời vẫn cho phép chỉnh sửa trước khi lưu.

#### 3.2. Dữ liệu tài chính bị phân tán
Thông tin có thể nằm rải rác ở hóa đơn giấy, ghi chú, ví điện tử và ứng dụng ngân hàng. Snaptics tập trung các giao dịch vào một hệ thống duy nhất, giúp người dùng có cái nhìn tổng quan hơn về dòng tiền.

#### 3.3. Khó kiểm soát ngân sách
Nhiều người chỉ nhận ra việc chi tiêu quá mức sau khi ngân sách đã bị vượt. Hệ thống cần theo dõi mức sử dụng, hiển thị tiến độ và gửi cảnh báo theo các ngưỡng được cấu hình.

#### 3.4. Thiếu khả năng phân tích hành vi chi tiêu
Các giao dịch riêng lẻ chỉ có giá trị hạn chế nếu không được tổng hợp. Snaptics phân tích dữ liệu theo thời gian và danh mục để chỉ ra khoản chi nổi bật, xu hướng tăng giảm và các gợi ý tham khảo.

#### 3.5. Khó phối hợp chi tiêu trong gia đình
Khi nhiều thành viên cùng đóng góp và sử dụng một nguồn tiền, dữ liệu cần được cập nhật tập trung. Ví và ngân sách dùng chung giúp các thành viên theo dõi số đã chi, số còn lại và lịch sử phát sinh.

#### 3.6. OCR và AI có thời gian xử lý dài
Nếu OCR và AI được xử lý hoàn toàn trong một request đồng bộ, hệ thống dễ timeout hoặc nghẽn khi nhiều người dùng quét hóa đơn. Amazon SQS và AI Worker tách tác vụ nặng khỏi luồng request chính.

#### 3.7. Quản lý tác vụ nền theo lịch
Các tác vụ như tạo insight định kỳ, kiểm tra ngân sách hoặc gửi thông báo cần được lập lịch và có thể chạy thủ công để kiểm thử. Hangfire cung cấp cơ chế quản lý lịch chạy trong Backend và giao diện Admin.

---

### 4. Kiến trúc giải pháp

Snaptics sử dụng kiến trúc Cloud-Native trên AWS, kết hợp Web SPA, CDN và lớp bảo vệ biên, container trên ECS Fargate, cơ sở dữ liệu quan hệ, lưu trữ đối tượng, hàng đợi bất đồng bộ và các dịch vụ AI bên ngoài. Hạ tầng mục tiêu được trải trên hai Availability Zone, tách biệt Frontend, Backend API và AI Worker để từng thành phần có thể triển khai, mở rộng và giám sát độc lập.

#### 4.1. Nguyên tắc thiết kế
* Tách tác vụ OCR/AI khỏi request chính để giảm timeout và tránh làm nghẽn Backend API.
* Triển khai VPC trên hai Availability Zone; đặt Backend, Worker và Database trong Private Subnet, chỉ công khai CloudFront, ALB, NAT Gateway và các điểm truy cập cần thiết.
* Lưu ảnh hóa đơn trên Amazon S3, truy cập nội bộ qua S3 Gateway Endpoint; lưu dữ liệu nghiệp vụ trong Amazon RDS for SQL Server.
* Gắn AWS WAF với CloudFront, quản lý JWT secret và connection string bằng AWS Secrets Manager; áp dụng quyền truy cập tối thiểu.
* Theo dõi log, metric, lỗi hàng đợi và chi phí ngay từ giai đoạn demo.
* Phân biệt kiến trúc mục tiêu production với cấu hình demo tối ưu chi phí.

#### 4.2. Thành phần giải pháp

| Thành phần | Vai trò trong hệ thống |
| :--- | :--- |
| **Frontend** | Angular Single Page Application được build và deploy tự động qua AWS Amplify từ GitHub Repository. CloudFront phân phối nội dung Frontend đến người dùng. |
| **DNS, CDN và bảo vệ biên** | Amazon Route 53 quản lý tên miền và phân giải DNS; Amazon CloudFront làm CDN và điểm vào chung. CloudFront chuyển tiếp các request API qua Internet Gateway đến ALB. |
| **Mạng (VPC)** | Amazon VPC trải trên 02 Availability Zone (AZ). Mỗi AZ có Public Subnet và Private Subnet; ALB và NAT Gateway thuộc tầng Public Subnet, còn ECS Fargate và Database thuộc tầng Private Subnet. |
| **Backend API (ECS Cluster)** | .NET API được đóng gói Docker, lưu trên Amazon ECR và triển khai thành ECS Service chạy Fargate Task trong Private Subnet của hai Availability Zone, nhận lưu lượng từ Application Load Balancer. |
| **AI Worker** | ECS Fargate Worker nhận message từ SQS `snaptics-ai-queue`, đọc ảnh từ S3 qua Gateway Endpoint, xử lý các tác vụ tự động qua NAT Gateway, sau đó lưu kết quả vào Aurora & RDS. |
| **Cơ sở dữ liệu (Database)** | **Aurora & RDS (Primary / Standby)** lưu dữ liệu người dùng, giao dịch, ví, ngân sách, thông báo, chat và ticket. Dữ liệu được đồng bộ liên tục giữa Primary (AZ 2) và Standby (AZ 1) trên mạng Private. |
| **Lưu trữ hóa đơn (Storage)** | Amazon S3 lưu ảnh hóa đơn và tệp xử lý; database chỉ lưu metadata và đường dẫn. Fargate Task truy cập S3 qua **S3 Gateway Endpoint** trực tiếp trong VPC để tối ưu chi phí và bảo mật. |
| **Hàng đợi (Messaging)** | **Amazon SQS (`snaptics-ai-queue`)** tiếp nhận các tác vụ OCR/AI bất đồng bộ; **Dead Letter Queue (DLQ)** tiếp nhận và giữ lại các message xử lý thất bại vượt quá số lần retry. |
| **Cấu hình & Bảo mật** | **AWS Secrets Manager** lưu trữ mã hóa RDS connection string và JWT secrets. IAM Roles và Security Groups kiểm soát quyền tối thiểu. |
| **Giám sát & Quản lý** | **Amazon CloudWatch** (thu thập log/metric), **AWS Secrets Manager** (quản lý secret), **AWS Budgets** (cảnh báo ngân sách) và **Simple Notification Service (SNS)** (phát thông báo sự cố/vận hành). |
| **CI/CD Pipeline** | GitHub Actions thực hiện 3 luồng: (1) **Auto Build & Deploy** Frontend lên AWS Amplify, (2) **Build & Push Docker Images** lên Elastic Container Registry (ECR), và (3) **Update Service** lên ECS Cluster để Fargate kéo image mới (**Pull Image**). |

#### 4.3. Luồng hoạt động chính theo sơ đồ kiến trúc
1. Người dùng truy cập tên miền Snaptics; **Amazon Route 53** (1) phân giải tên miền đến **Amazon CloudFront** (2).
2. **CloudFront** phân phối giao diện được triển khai trên **AWS Amplify**. **AWS WAF** gắn với CloudFront kiểm tra và chặn request bất thường trước khi request API đi vào hệ thống.
3. Request API được chuyển qua **Internet Gateway** đến **Application Load Balancer (ALB)** (3) đặt trong Public Subnet.
4. **Application Load Balancer** phân phối request đến **Backend API** (4) chạy bằng **Amazon ECS Fargate** trong Private Subnet của hai Availability Zone.
5. Backend lưu hoặc đọc ảnh hóa đơn trên **Amazon S3** thông qua **S3 Gateway Endpoint** (5), không cần đưa lưu lượng S3 đi qua Internet.
6. Backend và AI Worker đọc/ghi dữ liệu nghiệp vụ trên **Amazon RDS for SQL Server** (6); kiến trúc mục tiêu đồng bộ dữ liệu từ Primary sang Standby Multi-AZ.
7. Backend đẩy tác vụ OCR/AI vào **Amazon SQS** (`snaptics-ai-queue`) (7); AI Worker nhận message, message lỗi vượt retry được chuyển sang **Dead Letter Queue**. ECS Service kéo Docker Image từ **Amazon ECR** khi triển khai.
8. Các Fargate Task trong Private Subnet gửi request xử lý ra ngoài (8).
9. Lưu lượng đi qua **NAT Gateway** (9) trong Public Subnet kết nối với Internet Gateway ra môi trường ngoài.
10. AI Worker xử lý các tác vụ tự động (10) qua kết nối Internet; các thông tin nhạy cảm được lấy an toàn từ **AWS Secrets Manager**.

#### 4.4. Sơ đồ kiến trúc tổng thể

![Sơ đồ kiến trúc hệ thống Snaptics trên AWS](/images/2-Proposal/snaptics_architecture.png)
*Hình 1. Kiến trúc mục tiêu của hệ thống Snaptics trên AWS*

#### 4.5. Bảo mật, giám sát và kiểm soát chi phí
* Sử dụng HTTPS và access token; gắn AWS WAF với CloudFront để lọc request bất thường ở lớp biên.
* Phân quyền Admin/User và kiểm tra quyền sở hữu dữ liệu tại Backend.
* Không ghi API key hoặc connection string trong mã nguồn/Docker Image; lưu JWT secret và RDS connection string trong AWS Secrets Manager.
* Đặt Backend, AI Worker và RDS SQL Server trong Private Subnet; chỉ ALB và NAT Gateway nằm ở Public Subnet, kèm Security Group giới hạn luồng truy cập.
* Truy cập Amazon S3 qua S3 Gateway Endpoint; đồng thời kiểm tra loại tệp, kích thước và định dạng hóa đơn trước khi xử lý.
* Ghi CloudWatch Logs cho lỗi hệ thống, tác vụ AI, hoạt động Admin, trạng thái Hangfire và luồng xử lý SQS/DLQ.
* Thiết lập CloudWatch Alarm cho ALB/ECS/RDS, theo dõi SQS Queue Depth và DLQ; dùng Amazon SNS và AWS Budgets để gửi cảnh báo.
* Giới hạn ECS Auto Scaling, thời gian chạy NAT/RDS/Fargate/WAF và thời gian tồn tại của tài nguyên demo để tránh phát sinh ngoài kế hoạch.

---

### 5. Timeline dự án (13 tuần)

| Thời gian | Trọng tâm | Công việc chính | Kết quả |
| :--- | :--- | :--- | :--- |
| **Tuần 1** | Nghiên cứu đề tài và AWS Cloud | Tìm hiểu yêu cầu dự án; khảo sát khó khăn trong quản lý chi tiêu; nghiên cứu mô hình SaaS và các dịch vụ AWS phù hợp. | Xác định lĩnh vực, định hướng Cloud và phạm vi nghiên cứu ban đầu. |
| **Tuần 2** | Khảo sát yêu cầu và thiết kế sơ bộ | Phân tích nhu cầu quét hóa đơn, giao dịch, ví, ngân sách và báo cáo; nghiên cứu OCR/AI; lựa chọn Angular, .NET, SQL Server và kiến trúc AWS sơ bộ. | Hoàn thành danh sách yêu cầu, công nghệ và sơ đồ kiến trúc ban đầu. |
| **Tuần 3** | Hình thành ý tưởng Snaptics | Chốt tên dự án, nhóm người dùng User/Admin, chức năng cốt lõi và phạm vi phiên bản demo; xây dựng backlog mức cao. | Hoàn thiện ý tưởng, đối tượng sử dụng và phạm vi sản phẩm. |
| **Tuần 4** | Khởi tạo mã nguồn | Tạo GitHub Repository; khởi tạo Frontend Angular và Backend .NET; tổ chức cấu trúc mã nguồn, branch và quy trình cập nhật. | Repository và cấu trúc dự án sẵn sàng cho phát triển. |
| **Tuần 5** | Giao dịch và danh mục | Phát triển API và giao diện tạo, sửa, xóa, tra cứu giao dịch; quản lý danh mục thu/chi. | Người dùng quản lý được giao dịch và danh mục cơ bản. |
| **Tuần 6** | Ví và ngân sách | Phát triển ví cá nhân, ví gia đình, thành viên dùng chung, ngân sách cá nhân/gia đình và logic tính mức sử dụng. | Hoàn thiện quản lý ví, ngân sách và dữ liệu dùng chung. |
| **Tuần 7** | Dashboard và lưu trữ hóa đơn | Xây dựng Dashboard, biểu đồ, báo cáo; phát triển giao diện quét/tải ảnh; tích hợp Amazon S3 để lưu hóa đơn. | Có báo cáo trực quan và quy trình lưu ảnh trên S3. |
| **Tuần 8** | OCR, AI và thông báo | Xây dựng module xử lý hóa đơn tự động; chuẩn hóa kết quả; xây dựng AI Insight và thông báo trong ứng dụng. | Hệ thống trích xuất hóa đơn, tạo giao dịch và cung cấp gợi ý AI. |
| **Tuần 9** | DLQ và Hangfire | Tạo Dead Letter Queue và AI Worker; chuyển xử lý OCR/AI sang bất đồng bộ; tích hợp HangfireController và trang Admin cấu hình lịch chạy. | Tác vụ AI xử lý qua queue; tác vụ định kỳ được quản lý từ Admin. |
| **Tuần 10** | Frontend, Secrets và Database AWS | Kết nối Amplify với GitHub; triển khai Frontend; lưu JWT secret và connection string trong Secrets Manager; tạo RDS SQL Server cho môi trường demo. | Frontend hoạt động trên AWS; database và toàn bộ cấu hình nhạy cảm được quản lý an toàn. |
| **Tuần 11** | VPC, S3 Endpoint, SQS, ECR và Container | Tạo VPC hai Availability Zone, Public/Private Subnet, Internet Gateway, NAT Gateway, S3 Gateway Endpoint và SQS/DLQ; đóng gói Backend/Worker; tạo ECR và GitHub Actions build/push image. | Hạ tầng mạng, endpoint, queue và kho container sẵn sàng cho triển khai Backend/Worker. |
| **Tuần 12** | Triển khai ECS Fargate | Tạo ECS Cluster/Service; triển khai Backend và AI Worker; cấu hình ALB, health check, Auto Scaling, quyền đọc Secrets Manager, CloudWatch, SNS và AWS Budgets. | Backend và Worker hoạt động trên Fargate, truy cập đúng secret, có giám sát và cảnh báo chi phí. |
| **Tuần 13** | Hoàn thiện, kiểm thử và demo | Cấu hình Route 53, CloudFront và AWS WAF; kiểm thử responsive, phân quyền, S3 Endpoint, SQS-Worker-DLQ, RDS, log, CI/CD và chi phí. | Phiên bản Snaptics theo kiến trúc cập nhật sẵn sàng trình bày và thử nghiệm. |

---

### 6. Ngân sách dự kiến

Ngân sách được lập cho 01 tháng phát triển, tích hợp và demo. Đây là dự toán theo cấu hình thu gọn; chi phí thực tế phụ thuộc thời gian tài nguyên tồn tại, lưu lượng, số trang OCR, số token AI và chính sách giá tại thời điểm sử dụng. Mức quy đổi phục vụ lập kế hoạch là 1 USD = 26.000 đồng.

#### 6.1. Giả định lập dự toán

| Thông số | Giả định |
| :--- | :--- |
| **Người dùng thử nghiệm** | Khoảng 100 người dùng |
| **Khối lượng hóa đơn** | 1.000 hóa đơn hoặc trang OCR trong tháng demo |
| **Lưu trữ S3** | Khoảng 20 GB ảnh và tệp xử lý |
| **Lưu lượng Frontend/CDN** | Khoảng 30-50 GB/tháng |
| **Backend và AI Worker** | Cấu hình nhỏ; tổng khoảng 200-220 task-hour trong giai đoạn tích hợp/demo |
| **Database** | RDS for SQL Server Express, Single-AZ, khoảng 20 GB |
| **Kết nối Internet từ Private Subnet** | 01 NAT Gateway, chỉ duy trì trong thời gian cần thiết |
| **Phạm vi** | Không bao gồm tên miền mua mới, thuế, AWS WAF chạy liên tục và vận hành production Multi-AZ |

#### 6.2. Bảng dự toán chi phí môi trường demo

| STT | Hạng mục dịch vụ | Cơ sở ước tính | Chi phí (USD) |
| :---: | :--- | :--- | :---: |
| **1** | AWS Amplify, CloudFront và Route 53 | Build/hosting Frontend, CDN lưu lượng thấp và 01 Hosted Zone | $4.50 |
| **2** | Amazon S3 | Lưu khoảng 20 GB ảnh hóa đơn và request upload/download | $1.00 |
| **3** | ECS Fargate - Backend và AI Worker | Task cấu hình nhỏ, tổng khoảng 200-220 giờ chạy | $8.00 |
| **4** | Application Load Balancer | Hoạt động trong giai đoạn triển khai và demo, lưu lượng thấp | $7.00 |
| **5** | Amazon RDS for SQL Server | SQL Server Express, Single-AZ, 20 GB | $20.00 |
| **6** | NAT Gateway và dữ liệu xử lý | 01 NAT Gateway, bật giới hạn trong thời gian tích hợp | $13.00 |
| **7** | Amazon SQS, SNS và ECR | Queue OCR/AI, cảnh báo cơ bản và lưu Docker Image | $1.00 |
| **8** | CloudWatch, Secrets Manager và AWS Budgets | Log, metric, alarm, lưu secret/API key và cảnh báo ngân sách | $3.00 |

#### 6.3. Cách kiểm soát chi phí
* Tạo AWS Budget và cảnh báo khi chi phí đạt 50%, 80% và 100% ngân sách.
* Chỉ duy trì NAT Gateway, ALB, RDS, Fargate và AWS WAF trong thời gian tích hợp, kiểm thử hoặc demo cần thiết.
* Đặt giới hạn tối thiểu/tối đa cho ECS Auto Scaling và không để AI Worker chạy dư thừa.
* Giới hạn dung lượng ảnh; nén ảnh trước khi tải lên khi phù hợp.
* Thiết lập thời gian lưu log CloudWatch và Lifecycle Policy cho tệp S3.
* Không gọi lại xử lý tự động khi giao dịch đã có kết quả hợp lệ; không ghi secret vào log.
* Xóa tài nguyên thử nghiệm ngay sau khi hoàn tất demo.

#### 6.4. Cơ sở tham khảo đơn vị tính phí
Dự toán sử dụng mô hình pay-as-you-go và các đơn vị tính phí do nhà cung cấp công bố. Các con số AWS trong bảng là mức kế hoạch nội bộ theo cấu hình demo; trước khi tạo tài nguyên, nhóm cần nhập thông số thực tế vào AWS Pricing Calculator để xác nhận lại.

* [AWS Amplify Pricing](https://aws.amazon.com/amplify/pricing/)
* [AWS Fargate Pricing](https://aws.amazon.com/fargate/pricing/)
* [Amazon Route 53 Pricing](https://aws.amazon.com/route53/pricing/)
* [Amazon VPC / NAT Gateway Pricing](https://aws.amazon.com/vpc/pricing/)
* [Elastic Load Balancing Pricing](https://aws.amazon.com/elasticloadbalancing/pricing/)
* [AWS WAF Pricing](https://aws.amazon.com/waf/pricing/)
* [AWS Secrets Manager Pricing](https://aws.amazon.com/secrets-manager/pricing/)
* [Amazon RDS for SQL Server Pricing](https://aws.amazon.com/rds/sqlserver/pricing/)

---

### 7. Rủi ro dự án & Biện pháp giảm thiểu

| STT | Rủi ro | Tác động | Khả năng | Mức độ | Biện pháp giảm thiểu & Kế hoạch dự phòng |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **1** | Sai lệch OCR do ảnh mờ, thiếu sáng hoặc bố cục hóa đơn không đồng nhất | Cao | Trung bình | **Cao** | Cho phép xem ảnh gốc, chỉnh sửa trước khi lưu, kiểm tra ngày/số tiền và đánh dấu trường có độ tin cậy thấp. |
| **2** | Dịch vụ xử lý tự động lỗi hoặc bị giới hạn request | Cao | Trung bình | **Cao** | Xử lý qua SQS; retry có backoff; giới hạn số lần thử; DLQ; tách lớp tích hợp; cho phép nhập thủ công. |
| **3** | Rò rỉ dữ liệu tài chính, connection string hoặc API key | Rất cao | Thấp | **Cao** | HTTPS; AWS WAF; Secrets Manager cho JWT secret và connection string; IAM tối thiểu; kiểm tra quyền sở hữu; Private Subnet; không ghi secret vào log. |
| **4** | Chi phí Cloud và AI tăng ngoài dự kiến | Cao | Trung bình | **Cao** | AWS Budgets; giới hạn Auto Scaling; giới hạn ảnh và request AI; log retention; xóa tài nguyên demo sau khi sử dụng. |
| **5** | Tồn đọng message khi nhiều người dùng quét cùng lúc | Trung bình | Trung bình | **Trung bình** | Theo dõi Queue Depth; tăng Worker trong giới hạn; hiển thị trạng thái xử lý; DLQ; tách Backend và Worker. |
| **6** | Phiên bản mới gây lỗi khi triển khai | Trung bình | Trung bình | **Trung bình** | CI/CD build kiểm tra; health check; lưu image ổn định trên ECR; tách cấu hình Dev/Prod; theo dõi CloudWatch. |
| **7** | AI Insight chung chung hoặc không phù hợp hoàn cảnh người dùng | Trung bình | Trung bình | **Trung bình** | Chỉ dùng giao dịch đã xác nhận; trình bày dưới dạng tham khảo; không quyết định tài chính thay người dùng; thu thập phản hồi. |
| **8** | Hangfire chạy sai lịch, trùng tác vụ hoặc không hoàn thành | Trung bình | Trung bình | **Trung bình** | Kiểm tra thời gian; chỉ Admin được cấu hình; hạn chế chạy đồng thời; hiển thị lần chạy gần nhất; ghi log và cho phép chạy lại. |
| **9** | Phạm vi chức năng vượt quá thời gian 13 tuần | Cao | Trung bình | **Cao** | Ưu tiên luồng cốt lõi; khóa phạm vi demo; chia backlog bắt buộc/tùy chọn; kiểm thử sớm và lùi chức năng không thiết yếu. |

---

### 8. Kết luận và kết quả kỳ vọng

Snaptics hướng đến việc chuyển đổi quản lý chi tiêu từ nhập liệu thủ công sang quy trình tự động, tập trung và có khả năng phân tích. Việc kết hợp Amazon SQS/DLQ, Hangfire, ECS Fargate, RDS SQL Server, S3 Gateway Endpoint, CloudFront/WAF và Secrets Manager giúp hệ thống xử lý hóa đơn an toàn hơn, giảm phụ thuộc vào thao tác nhập liệu và tạo nền tảng cho các tính năng tài chính trong tương lai.

Sau 13 tuần, dự án kỳ vọng hoàn thành phiên bản demo có thể trình bày đầy đủ luồng từ quét hóa đơn đến tạo giao dịch, cập nhật ví/ngân sách, hiển thị báo cáo và gửi thông báo. Đồng thời, nhóm có thể chứng minh khả năng triển khai Frontend, Backend, Worker, Database, Storage, Queue, CI/CD và Monitoring trên môi trường Cloud.

| Nhóm kết quả | Kết quả kỳ vọng |
| :--- | :--- |
| **Sản phẩm** | Có phiên bản Web hoạt động theo phạm vi User/Admin và luồng nghiệp vụ cốt lõi. |
| **Kỹ thuật** | Thể hiện kiến trúc CloudFront/WAF, container trên ECS Fargate, queue bất đồng bộ, S3 Gateway Endpoint, RDS SQL Server và xử lý tác vụ tự động. |
| **Vận hành** | Có health check, CloudWatch log/metric, DLQ, Secrets Manager, cảnh báo chi phí và cơ chế quản lý tác vụ nền. |
| **Khả năng mở rộng** | Kiến trúc mục tiêu có thể chuyển từ demo thu gọn sang production Multi-AZ sau khi xác nhận tải và ngân sách. |