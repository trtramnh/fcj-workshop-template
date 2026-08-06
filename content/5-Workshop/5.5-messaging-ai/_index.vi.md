---
title: "AI & Tác vụ nền (Messaging)"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---


Trái tim sức mạnh của Snaptics nằm ở khả năng xử lý bất đồng bộ. Việc gọi API bóc tách ảnh hóa đơn có thể mất từ 5-10 giây, do đó ta không thể bắt người dùng ngồi nhìn vòng tròn xoay loading. Thay vào đó, ta sẽ quăng tác vụ đó vào hàng đợi SQS.

## 1. Cấu hình SQS & Dead Letter Queue 

Trong môi trường Enterprise, chỉ tạo 1 Queue là không đủ an toàn. Giả sử API AI của Azure bị sập tạm thời, hệ thống đọc hóa đơn thất bại, message đó sẽ cứ bị đẩy lại vào Queue và gây ra vòng lặp lỗi vô tận (Infinite Loop). Để chặn đứng việc này, ta phải tạo **Dead Letter Queue (DLQ)**.

### A. Tạo Hàng đợi chết (DLQ)
- Vào **Amazon SQS ➔ Create queue**.
- **Type:** Standard.
- **Name:** Đặt là `snaptics-ai-queue-dlq` (Có hậu tố DLQ).
- Các cài đặt khác giữ nguyên, bấm Create.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.5-messaging-ai/amazon_sqs_create.png" >
  </div>

### B. Tạo Main Queue 
- Quay lại và bấm **Create queue**.
- **Type:** Standard.
- **Name:** Đặt chính xác là `snaptics-ai-queue` (Khớp 100% với sơ đồ kiến trúc).
- **Visibility timeout:** `1 minutes`.
- **Dead-letter queue:** Mở khóa phần này ra, bật **Enabled**.
- Chọn DLQ vừa tạo ở bước A.
- **Maximum receives:** Đặt là `3`. *(Điều này có nghĩa là: Code C# trên ECS chỉ được phép thử quét hóa đơn lỗi tối đa 3 lần. Nếu lần thứ 3 vẫn thất bại do AI sập, SQS sẽ tự động hất cái message lỗi đó sang chuồng DLQ để cách ly, giúp hệ thống không bị kẹt).*
- Bấm Create.

## 2. Tích hợp AI 

ECS Container sẽ móc message từ Queue ra và âm thầm gọi sang 2 con AI hàng đầu thế giới để xử lý:

### Azure Document Intelligence - OCR
Khi ảnh hóa đơn nằm trên S3, AWS sẽ gọi Azure để bóc tách text:
```csharp
// Lấy Key bảo mật từ AWS Systems Manager thông qua IConfiguration
var azureKey = _config["AiSettings:AzureDocIntelKey"];
var azureEndpoint = _config["AiSettings:AzureDocIntelEndpoint"];
var credential = new AzureKeyCredential(azureKey);
var client = new DocumentAnalysisClient(new Uri(azureEndpoint), credential);

// Phân tích ảnh hóa đơn (Ảnh được gọi qua đường hầm VPC Endpoint an toàn)
AnalyzeDocumentOperation operation = await client.AnalyzeDocumentFromUriAsync(WaitUntil.Completed, "prebuilt-receipt", new Uri(preSignedUrl));
var result = operation.Value;

// Nhặt tên Cửa hàng và Giá tiền
string merchantName = result.Documents[0].Fields["MerchantName"].Value.AsString();
double total = result.Documents[0].Fields["Total"].Value.AsDouble();
```

### Google Gemini (Nhận diện siêu tốc & Tư vấn)
Snaptics sử dụng model Gemini thế hệ mới nhất (Gemini 2.5 Flash) để trích xuất hóa đơn siêu tốc và tư vấn tài chính thông minh:
```csharp
var requestBody = new {
    contents = new[] { new { parts = new[] { new { text = userPrompt } } } }
};

var apiKey = _config["AiSettings:GeminiApiKey"];
var modelName = _config["AiSettings:GeminiModel"]; // Ví dụ: gemini-2.5-flash
var endpoint = $"https://generativelanguage.googleapis.com/v1beta/models/{modelName}:generateContent?key={apiKey}";

var response = await _httpClient.PostAsJsonAsync(endpoint, requestBody);
```

## 3. Quản lý Tác vụ định kỳ (Hangfire)

Bên cạnh SQS xử lý sự kiện tức thời, Snaptics còn dùng Hangfire để chạy các Cron Job định kỳ (ví dụ: Chốt sổ tài chính lúc nửa đêm).

```csharp
// Hangfire tận dụng luôn cụm SQL Server Database siêu mạnh để lưu trạng thái Job
builder.Services.AddHangfire(config => config
    .UseSqlServerStorage(builder.Configuration.GetConnectionString("DefaultConnection")));
builder.Services.AddHangfireServer();

// Chạy tác vụ tự động lúc 00:00 ngày mùng 1 hàng tháng
RecurringJob.AddOrUpdate<IReportService>(
    "monthly-ai-insight",
    job => job.GenerateMonthlyInsightsAsync(),
    "0 0 1 * *"); // Cú pháp Cron
```

Sự kết hợp hoàn hảo giữa SQS (Cho luồng dữ liệu biến động tức thời) và Hangfire (Cho lịch trình cố định) giúp kiến trúc Backend của Snaptics vận hành trơn tru như một cỗ máy công nghiệp!