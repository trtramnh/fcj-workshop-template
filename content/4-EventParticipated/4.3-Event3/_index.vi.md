---
title: "Event 3"
date: 2026-07-25
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# FCAJ – Agentic AI Build Week Sharing Session

---

### Mục Đích Của Sự Kiện

Sự kiện **FCAJ – Agentic AI Build Week Sharing Session** được tổ chức nhằm mang lại những góc nhìn thực tiễn từ các đội thi đã tham gia cuộc thi FCAJ Agentic AI Build Week.

Nội dung buổi chia sẻ tập trung giải thích quy trình lên kế hoạch, triển khai và vận hành các ứng dụng Agentic AI trên hạ tầng AWS. Sự kiện giới thiệu các kiến trúc đám mây hiện đại, các quyết định kỹ thuật quan trọng, cùng những bài học kinh nghiệm về làm việc nhóm, giải quyết vấn đề và phát triển phần mềm trong khoảng thời gian giới hạn.

---

### Danh Sách Diễn Giả

- **Diễn giả Quốc tế** – Chuyên gia Công nghệ & Lãnh đạo Công nghiệp
- **One Team** – Đại diện Đội thi FCAJ Build Week
- **Plan V** – Đại diện Đội thi FCAJ Build Week
- **3KA** – Đại diện Đội thi FCAJ Build Week
- **Dream AI Team** – Đại diện Đội thi FCAJ Build Week
- **Six Pillar Team** – Đại diện Đội thi FCAJ Build Week

---

### Nội Dung Nổi Bật

#### 1. Phiên Khai Mạc & Xu Hướng Công Nghệ
* Diễn giả quốc tế chia sẻ những trải nghiệm thực tế trong quá trình làm việc tại các tập đoàn công nghệ lớn.
* Phân tích sự phát triển nhanh chóng của Trí tuệ Nhân tạo và cách công nghệ mới đang làm thay đổi phương thức phát triển hệ thống phần mềm.
* Khuyến khích người tham dự giữ vững tinh thần tò mò, chủ động khám phá các công nghệ mới và xây dựng thói quen học tập liên tục thay vì chỉ tập trung vào kỹ năng kỹ thuật cố định.
* Nhấn mạnh khả năng thích ứng và tự hoàn thiện bản thân là yếu tố quan trọng hàng đầu cho sinh viên và kỹ sư muốn phát triển trong lĩnh vực Điện toán Đám mây và AI.

#### 2. Hành Trình Phát Triển & Các Bài Toán Thực Tế
* Các đội thi trình trình bày chi tiết toàn bộ quá trình phát triển sản phẩm trong cuộc thi Buildathon, từ việc xác định bài toán thực tế, lý do chọn đề tài, thiết kế kiến trúc hệ thống, xử lý khó khăn kỹ thuật đến các bài học rút ra.
* **Hệ thống đặt món ăn tự phục vụ thông minh (Self-service Food Ordering)**: Ứng dụng kiosk AI hỗ trợ khách hàng tương tác và đặt hàng bằng ngôn ngữ tự nhiên.
* **Ứng dụng AI trong Ngành Tài chính**: Tự động hóa quy trình giao tiếp với khách hàng và xử lý các giao dịch tài chính.

#### 3. Chuyên Sâu Kiến Trúc Cloud & Dịch Vụ AWS Managed
* Các đội thi phân tích chi tiết cách lựa chọn và kết hợp các dịch vụ AWS managed nhằm hỗ trợ lập luận AI, xử lý backend, lưu trữ, phân phối ứng dụng, bảo mật và giám sát:
  * **Amazon Bedrock & Amazon Bedrock AgentCore**: Đảm nhận khả năng lập luận AI, điều phối Agent (Agent Orchestration) và Generative AI.
  * **Amazon API Gateway & AWS Lambda**: Xây dựng hệ thống backend serverless và tích hợp các RESTful APIs.
  * **Amazon SQS**: Quản lý giao tiếp bất đồng bộ và xử lý các tác vụ chạy ngầm (background jobs).
  * **Amazon DynamoDB & Amazon S3**: Lưu trữ dữ liệu cấu trúc (giao dịch, tài khoản) và dữ liệu phi cấu trúc (hình ảnh hóa đơn, tài liệu).
  * **Amazon OpenSearch Service**: Hỗ trợ tìm kiếm ngữ nghĩa (semantic search) và truy xuất dữ liệu dạng vector cho mô hình RAG.
  * **Amazon ECS, AWS Fargate & Amazon ECR**: Đóng gói container và triển khai ứng dụng microservices linh hoạt.
  * **Amazon CloudFront, Amazon Cognito & Application Load Balancer (ALB)**: Đảm bảo khả năng truy cập ứng dụng an toàn, tốc độ cao và xác thực người dùng.
  * **Amazon CloudWatch & AWS CloudTrail**: Ghi log hệ thống, giám sát hiệu năng theo thời gian thực và theo dõi nhật ký hoạt động.
  * **AWS WAF, GuardDuty, IAM, KMS & Secrets Manager**: Bảo vệ ứng dụng web, quản lý quyền truy cập, mã hóa dữ liệu và quản lý khóa/credential an toàn.

---

### Những Gì Học Được

#### Kiến Thức Chuyên Môn & Kiến Trúc Cloud-Native
* **Thiết kế hệ thống AI chuẩn Cloud-Native**: Hiểu rõ cách kết hợp nhiều dịch vụ managed của AWS để tạo nên hệ thống Agentic AI hoàn chỉnh, linh hoạt và dễ bảo trì thay vì chỉ gọi mô hình AI đơn lẻ.
* **Kiến trúc tích hợp đa tầng**: Nắm vững phương pháp phối hợp giữa compute serverless, container, cơ sở dữ liệu vector, API gateway và các lớp bảo mật chuyên sâu.

#### Thực Tiễn Triển Khai & Quy Trình Kỹ Thuật
* **Tầm quan trọng của việc lập kế hoạch kiến trúc**: Thấy được lợi ích của việc thiết kế kiến trúc kỹ lưỡng trước khi bắt tay vào viết code, tính đến các yếu tố mở rộng (scalability), giám sát (monitoring) và bảo mật ngay từ đầu.
* **Xử lý thách thức dưới áp lực thời gian**: Học hỏi kinh nghiệm quản lý yêu cầu thay đổi, tích hợp API phức tạp và phân chia công việc trong mốc thời gian giới hạn của cuộc thi.

#### Phát Triển Bản Thân & Tinh Thần Làm Việc Nhóm
* Nâng cao nhận thức về tầm quan trọng của kỹ năng làm việc nhóm, giao tiếp hiệu quả và quy trình phát triển lặp (iterative development).
* Tạo động lực mạnh mẽ để tiếp tục trau dồi kiến thức AWS, kiến trúc đám mây và phát triển hệ thống backend.

---

### Khả Năng Áp Dụng Vào Dự Án Snaptics

#### 1. Hệ thống Backend Serverless Agentic với Bedrock AgentCore & Lambda
* Áp dụng **Amazon Bedrock AgentCore** và **AWS Lambda** kết hợp **API Gateway** vào **Snaptics** để xây dựng trợ lý tư vấn tài chính tự động.
* Trợ lý AI có thể phân tích câu hỏi ngôn ngữ tự nhiên của người dùng, tự động truy vấn dữ liệu chi tiêu và đưa ra lời khuyên tài chính phù hợp.

#### 2. Luồng xử lý bất đồng bộ & Đọc hóa đơn chạy ngầm
* Sử dụng **Amazon SQS** và **AWS Lambda** để xử lý các tác vụ đọc hóa đơn (OCR) và cập nhật giao dịch hàng loạt theo chế độ bất đồng bộ, giúp giao diện người dùng luôn phản hồi mượt mà.

#### 3. Lưu trữ đa mô hình & Tìm kiếm RAG với OpenSearch
* Lưu trữ bản ghi giao dịch cấu trúc trong **Amazon DynamoDB** và ảnh hóa đơn trong **Amazon S3**.
* Sử dụng **Amazon OpenSearch Service** để lưu trữ vector kiến thức tài chính và lịch sử chi tiêu, hỗ trợ mô hình RAG đưa ra khuyến nghị chính xác cho từng người dùng Snaptics.

#### 4. Bảo mật toàn diện, Quản lý Secret & Giám sát Hệ thống
* Áp dụng **AWS WAF**, **Amazon Cognito** và **AWS IAM** để bảo vệ API và phân quyền chi tiết cho người dùng Snaptics.
* Sử dụng **AWS Secrets Manager** và **AWS KMS** để mã hóa và quản lý an toàn các API keys/chuỗi kết nối CSDL.
* Giám sát hiệu năng và nhật ký bảo mật bằng **Amazon CloudWatch** và **AWS CloudTrail**.

---

### Trải Nghiệm Trong Event

Tham dự sự kiện **FCAJ – Agentic AI Build Week Sharing Session** tại Bitexco Financial Tower mang lại cho tôi cái nhìn thực tế về quy trình đưa một ý tưởng AI thành sản phẩm hoàn chỉnh trên AWS.

Những câu chuyện chia sẻ thực tế từ các đội thi giúp tôi nhận ra rằng phát triển ứng dụng AI thành công đòi hỏi sự kết hợp chặt chẽ giữa thiết kế hệ thống, hạ tầng an toàn, sự hợp tác hiệu quả giữa các thành viên và khả năng xử lý sự cố nhanh chóng.

---

### Bài Học Rút Ra

* Xây dựng giải pháp Agentic AI không chỉ dừng lại ở việc kết nối với mô hình LLM mà cần một hạ tầng đám mây đồng bộ bao gồm compute, serverless, lưu trữ vector, bảo mật và observability.
* Sự kết hợp giữa **Amazon Bedrock AgentCore**, **AWS Lambda**, **Amazon ECS/Fargate**, **DynamoDB** và **OpenSearch Service** tạo nên một mô hình kiến trúc chuẩn có thể áp dụng trực tiếp cho dự án **Snaptics**.
* Ưu tiên thiết kế kiến trúc, tuân thủ các nguyên tắc bảo mật và phát triển lặp là chìa khóa để hoàn thành dự án chất lượng trong thời gian ngắn.

---

### Một Số Hình Ảnh Khi Tham Gia Sự Kiện

Dưới đây là hình ảnh thực tế được chụp tại sự kiện **FCAJ – Agentic AI Build Week Sharing Session** tổ chức tại Bitexco Financial Tower, bao gồm các slide kiến trúc, bài trình bày khai mạc, không gian hội trường và ảnh check-in lưu niệm.

<div style="display: flex; gap: 15px; justify-content: center; align-items: flex-start; flex-wrap: wrap; margin-top: 20px;">
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.3-Event3/event3_1_workflow.jpg" alt="Slide luồng xử lý ứng dụng AI-Native & Retrieval" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Hình 1:</b> Slide trình bày luồng xử lý ứng dụng AI-Native (Knowledge Base, Bedrock Model, Draw.io MCP & AWS Pricing MCP).</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.3-Event3/event3_2_architecture.jpg" alt="Slide kiến trúc Agentic Cloud tối ưu chi phí" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Hình 2:</b> Slide sơ đồ kiến trúc Agentic AI tối ưu chi phí do đội thi Build Week chia sẻ (Route53, API Gateway, AgentCore Runtime, Subagents & Observability).</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.3-Event3/event3_4_opening.jpg" alt="Phiên phát biểu khai mạc từ diễn giả quốc tế" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Hình 3:</b> Diễn giả quốc tế phát biểu khai mạc sự kiện FCAJ – Agentic AI Build Week Sharing Session.</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.3-Event3/event3_5_venue.jpg" alt="Khung cảnh hội trường sự kiện tại Bitexco Financial Tower" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Hình 4:</b> Toàn cảnh không gian hội trường và người tham dự tại Bitexco Financial Tower.</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.3-Event3/event3_3_me.jpg" alt="Chụp ảnh lưu niệm tại backdrop AWS" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Hình 5:</b> Chụp ảnh check-in lưu niệm tại phông nền AWS First Cloud Journey AI.</p>
  </div>
</div>

---

> **Tổng kết:** Sự kiện FCAJ – Agentic AI Build Week Sharing Session mang đến những kiến thức giá trị về cách lên kế hoạch, phát triển, triển khai và bảo mật các hệ thống AI thực tế trên AWS. Sự kiện giúp tôi hiểu rõ các mô hình kiến trúc cloud-native, quy trình serverless/container, tiêu chuẩn bảo mật và tinh thần làm việc nhóm, tạo nền tảng vững chắc cho các dự án đám mây như Snaptics trong tương lai.
