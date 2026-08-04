---
title: "Agent Forge – Deep Dive Day 1"
date: 2026-08-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Agent Forge – Deep Dive Day 1: Foundations & Agent Setup

- **Thời gian**: 09:00 – 12:00, ngày 01/08/2026
- **Địa điểm**: Tầng 26, Bitexco Financial Tower, TP. Hồ Chí Minh

---

### Mục Đích Của Sự Kiện

Sự kiện được tổ chức nhằm cung cấp kiến thức nền tảng và định hướng chuyên sâu về cách xây dựng, triển khai và vận hành các hệ thống **Agentic AI** trên môi trường thực tế.

Thông qua phần giới thiệu về **Amazon Bedrock AgentCore**, người tham dự được tiếp cận các thành phần quan trọng phục vụ việc đưa AI Agent từ giai đoạn thử nghiệm lên môi trường production, bao gồm **Runtime**, **Gateway**, **Identity** và các cơ chế tích hợp công cụ.

---

### Danh Sách Diễn Giả

- **Nghĩa Trần** – Agentic Solutions Architect (SA)
- **Anh Phạm** – Cloud Consultant, G-AsiaPacific Vietnam

---

### Nội Dung Nổi Bật

#### 1. Tổng quan về Agentic AI
* Buổi học giới thiệu khái niệm AI Agent, khả năng tự chủ của agent và sự khác biệt giữa các luồng xử lý được lập trình cố định với những hệ thống có khả năng tự lập kế hoạch, lựa chọn công cụ và thực hiện nhiều bước để hoàn thành mục tiêu.
* Chương trình giới thiệu **Strands Agents SDK**, một SDK mã nguồn mở hỗ trợ xây dựng AI Agent theo hướng *model-driven*, giúp giảm bớt phần mã điều phối phức tạp.

#### 2. Amazon Bedrock AgentCore Runtime
* AgentCore Runtime cung cấp môi trường serverless chuyên biệt để triển khai và vận hành AI Agent.
* Mỗi phiên làm việc được chạy trong một **microVM** riêng biệt, giúp cô lập tài nguyên CPU, bộ nhớ và filesystem giữa các người dùng.
* Runtime hỗ trợ duy trì trạng thái trong cùng một session, xử lý bất đồng bộ và thực thi các tác vụ kéo dài.

#### 3. Amazon Bedrock AgentCore Identity
* **Inbound authentication**: Kiểm soát người dùng hoặc ứng dụng nào được phép gọi agent.
* **Outbound authentication**: Kiểm soát cách agent truy cập API, dịch vụ AWS hoặc nền tảng bên thứ ba.
* Agent được cấp một *workload identity* riêng và có thể sử dụng *workload access token* để truy cập tài nguyên theo đúng phạm vi được cho phép. Tích hợp linh hoạt với **Amazon Cognito** và các OAuth/JWT Identity Provider.

#### 4. Amazon Bedrock AgentCore Gateway
* Cung cấp một điểm truy cập tập trung và an toàn để AI Agent kết nối với các công cụ, API, Lambda function hoặc các agent khác.
* Chuyển đổi những API hiện có thành các công cụ tương thích với **Model Context Protocol (MCP)**, giúp agent khám phá và gọi công cụ theo chuẩn thống nhất.
* Hỗ trợ **semantic search** để tự động tìm công cụ phù hợp bằng câu lệnh ngôn ngữ tự nhiên khi số lượng API lớn.

#### 5. Hands-on Lab
* Triển khai một AI Agent cơ bản lên AgentCore Runtime.
* Kết nối agent với các công cụ bên ngoài và Knowledge Base.
* Xây dựng giao diện web tương tác trực tiếp với agent.
* Tích hợp **Amazon Cognito** nhằm xác thực người dùng trước khi cấp quyền truy cập agent.

---

### Những Gì Học Được

#### Kiến Thức Chuyên Môn
* **Production-Ready Agentic Architecture**: Việc xây dựng AI Agent cho production đòi hỏi kiến trúc hoàn chỉnh bao gồm cô lập dữ liệu (microVM), xác thực 2 chiều (Identity), quản lý API tập trung (Gateway) và giám sát chặt chẽ.
* **Model-Driven Development**: Tận dụng Strands SDK và MCP Server để tiêu chuẩn hóa việc gọi công cụ ngoài thay vì viết mã điều phối tùy biến phức tạp.

#### Quản Trị & Bảo Mật Hệ Thống
* **Bảo vệ kết nối & Dữ liệu**: Hạn chế truyền dữ liệu qua Internet công cộng bằng cách sử dụng **AWS PrivateLink**, mã hóa toàn bộ luồng truyền tải và áp dụng phân quyền tối thiểu (Least Privilege).

---

### Khả Năng Áp Dụng Vào Dự Án Snaptics

#### 1. Xây dựng AI Agent xử lý Insight tài chính
Thay vì chỉ gọi mô hình AI để trả về câu trả lời đơn lẻ, Snaptics có thể phát triển một Agent có khả năng:
* Phân tích dữ liệu giao dịch, ví và ngân sách.
* Đánh giá thói quen chi tiêu của người dùng và tự động chọn API phù hợp để lấy thêm dữ liệu.
* Tạo lời khuyên tài chính theo từng tình huống và gửi về hệ thống Notification.
* Phù hợp với luồng hiện tại: Sau khi người dùng lưu kết quả, hệ thống gọi API `/AiAssistant/insight` và đẩy thông báo cho người dùng.

#### 2. Quản lý API thông qua Gateway (MCP Server)
* Đóng gói các API giao dịch, ngân sách, ví, mục tiêu tiết kiệm và thông báo thành các công cụ (tools) riêng biệt.
* AgentCore Gateway đóng vai trò lớp trung gian kiểm soát quyền truy cập, đảm bảo Agent chỉ được gọi đúng API được cấp phép thay vì truy cập trực tiếp DB.

#### 3. Bảo vệ dữ liệu tài chính người dùng
* Kết hợp AgentCore Identity & Amazon Cognito để đảm bảo từng người dùng chỉ truy cập dữ liệu của chính mình.
* Quyền truy cập được giới hạn nghiêm ngặt theo từng công cụ và từng chức năng.

#### 4. Áp dụng Human-in-the-Loop (Xác nhận từ người dùng)
Đối với các thao tác nhạy cảm (điều chỉnh ngân sách, xóa/sửa giao dịch, thay đổi mục tiêu tiết kiệm, gửi thông báo thay mặt người dùng):
* AI chỉ đưa ra đề xuất và yêu cầu người dùng xác nhận trước khi thực thi.
* Giúp Snaptics tận dụng tối đa khả năng tự động hóa của AI nhưng vẫn giữ quyền quyết định cuối cùng cho người dùng.

---

### Trải Nghiệm Trong Event

Buổi workshop chuyên sâu với khối lượng kiến thức lớn, kết hợp giữa phần trình bày kiến trúc và hoạt động thực hành hands-on trực tiếp. Phần thực hành giúp tôi nắm vững toàn bộ luồng từ khởi tạo agent đơn giản đến việc chuẩn bị các hạ tầng cần thiết cho môi trường sản xuất.

---

### Bài Học Rút Ra

* Một AI Agent có khả năng suy luận tốt chưa đủ để trở thành một hệ thống production-ready. Agent cần được đặt trong một kiến trúc có các lớp kiểm soát rõ ràng về Runtime, Identity, Gateway, quyền truy cập và dữ liệu.
* Đối với Snaptics, AI không nên được trao quyền thao tác trực tiếp với toàn bộ dữ liệu tài chính. Mỗi khả năng của agent cần được giới hạn thành một công cụ cụ thể, có xác thực, phân quyền và yêu cầu xác nhận của người dùng đối với những hành động nhạy cảm.

---

### Một Số Hình Ảnh Khi Tham Gia Sự Kiện

Dưới đây là một số hình ảnh ghi lại quá trình tham gia sự kiện **AWS FCAJ Agent Forge – Deep Dive Day 1**, bao gồm phần chia sẻ kiến thức từ diễn giả, hoạt động thực hành triển khai AI Agent và quá trình trao đổi, học hỏi cùng các thành viên tham dự.

<div style="display: flex; gap: 15px; justify-content: center; align-items: flex-start; flex-wrap: wrap; margin-top: 20px;">
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.1-Event1/event1_1.jpg" alt="Không gian tổ chức sự kiện tại Bitexco Financial Tower" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Hình 1:</b> Không gian tổ chức sự kiện tại Bitexco Financial Tower.</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.1-Event1/event1_2.jpg" alt="Phần trình bày tổng quan về Amazon Bedrock AgentCore" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Hình 2:</b> Phần trình bày diễn giả về Agentic AI & AgentCore.</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.1-Event1/event1_3.jpg" alt="Tôi cùng các thành viên trong nhóm tại sự kiện Agent Forge – Deep Dive" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Hình 3:</b> Tôi cùng các thành viên trong nhóm tại sự kiện Agent Forge – Deep Dive.</p>
  </div>
  <div style="flex: 1; min-width: 220px; text-align: center;">
    <img src="/images/4.1-Event1/event1_4.jpg" alt="Hình ảnh lưu niệm cùng diễn giả và các thành viên tham dự" style="border-radius: 8px; width: 100%; height: auto; box-shadow: 0 4px 8px rgba(0,0,0,0.1);" />
    <p style="font-size: 0.9em; margin-top: 8px; color: #555;"><b>Hình 4:</b> Kỷ niệm cùng diễn giả và các thành viên tham dự sự kiện.</p>
  </div>
</div>

---

> **Tổng kết:** Sự kiện AWS FCAJ Agent Forge – Deep Dive Day 1 đã cung cấp một nền tảng kiến thức toàn diện về cách xây dựng và vận hành AI Agent trên AWS. Quan trọng hơn, sự kiện giúp tôi định hình hướng phát triển AI cho Snaptics: không chỉ tạo ra các lời khuyên thông minh, mà còn xây dựng một hệ thống AI có khả năng kết nối công cụ, xử lý bất đồng bộ, bảo vệ dữ liệu người dùng và hoạt động an toàn trong môi trường thực tế.
