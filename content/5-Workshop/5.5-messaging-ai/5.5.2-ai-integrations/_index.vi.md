---
title: "Tích hợp Trí tuệ Nhân tạo (AI)"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---


Snaptics giao tiếp với 2 mô hình AI hàng đầu thông qua REST API (đã được lưu Key ở SSM Parameter Store).

### 1. Phân tích OCR bằng Azure Document Intelligence

Azure nổi tiếng với mô hình đọc biên lai (Pre-built Receipt Model). Dưới đây là cách mà Snaptics (bên trong Service của .NET) gọi Azure để quét bức ảnh hóa đơn nằm trên S3:

```csharp
// Sử dụng Azure.AI.FormRecognizer SDK
var credential = new AzureKeyCredential(_config["AiSettings:AzureDocIntelKey"]);
var client = new DocumentAnalysisClient(new Uri(_config["AiSettings:AzureDocIntelEndpoint"]), credential);

// Tạo Pre-signed URL từ file ảnh S3
string preSignedUrl = _s3Service.GeneratePreSignedUrl("s3-bucket-snaptics", s3Key, 10);

// Phân tích với model 'prebuilt-receipt'
AnalyzeDocumentOperation operation = await client.AnalyzeDocumentFromUriAsync(WaitUntil.Completed, "prebuilt-receipt", new Uri(preSignedUrl));
var result = operation.Value;

// Bóc tách tự động Merchant Name và Total Amount
string merchantName = result.Documents[0].Fields["MerchantName"].Value.AsString();
double total = result.Documents[0].Fields["Total"].Value.AsDouble();
```

### 2. Tư vấn Tài chính với Google Gemini

Google Gemini có tốc độ xử lý siêu nhanh (đặc biệt là model 1.5 Flash). Snaptics có một tính năng tính toán số dư cuối tháng và hỏi Gemini đưa ra nhận xét:

```csharp
var prompt = $"Tôi đã tiêu {totalExpense} VNĐ tháng này cho Giải trí và Ăn uống. " +
             $"Ngân sách của tôi là {budget} VNĐ. Hãy cho tôi lời khuyên ngắn gọn bằng tiếng Việt.";

// Gọi ra Gemini API Endpoint
var requestBody = new {
    contents = new[] { new { parts = new[] { new { text = prompt } } } }
};

var response = await _httpClient.PostAsJsonAsync(
    $"https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key={_config["AiSettings:GeminiApiKey"]}", 
    requestBody);
```
Với thiết kế VPC có `NAT Gateway`, các luồng gọi HTTP ra ngoài Azure và Google này hoạt động trơn tru và bảo mật.
