---
title: "SQS Queues & SNS Topics"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---


<!-- TODO: ChÃ¨n áº£nh mÃ n hÃ¬nh hÃ ng Ä‘á»£i Amazon SQS (snaptics-ai-queue) vÃ  Amazon SNS topic vÃ o Ä‘Ã¢y -->
![SQS Queue](/images/5-Workshop/placeholder-sqs.png)
![SNS Topic](/images/5-Workshop/placeholder-sns.png)
AWS SQS (Simple Queue Service) và SNS (Simple Notification Service) kết hợp với nhau tạo thành xương sống cho kiến trúc Event-Driven của Snaptics.

### 1. Tạo SQS Queue
- Mở **Amazon SQS ➔ Create queue**.
- **Type:** Standard.
- **Name:** `snaptics-main-queue`
- **Visibility timeout:** `1 minutes` (Thời gian giấu message để worker khác không xử lý trùng).
- Bấm **Create queue**. Copy **URL** (VD: `https://sqs.ap-southeast-1.amazonaws.com/123/snaptics-main-queue`).

### 2. Tạo SNS Topic
SNS dùng để gửi Push Notification (hoặc Email cảnh báo cho Admin) ngay lập tức tới nhiều nguồn đăng ký nhận (Subscriber) khác nhau.
- Mở **Amazon SNS ➔ Topics ➔ Create topic**.
- **Type:** Standard.
- **Name:** `snaptics-alerts`
- Bấm **Create topic**. 
- Vào Topic vừa tạo, bấm **Create subscription**, chọn Protocol là `Email`, nhập địa chỉ Email của bạn vào, sau đó mở hòm thư bấm Confirm. Từ giờ hệ thống có lỗi gì sẽ báo thẳng về Email.

### Tích hợp vào Snaptics (.NET C#)
Ứng dụng Snaptics sử dụng gói `AWSSDK.SQS` và Inject interface `IAmazonSQS` qua cơ chế Dependency Injection (DI) trong `Program.cs`. 

Khi có một User upload hóa đơn mới, thay vì gọi thẳng hàm OCR mất 5-10 giây, API chỉ đơn giản ném một message vào SQS:

```csharp
// Trong Controller xử lý Invoice
var requestMessage = new SendMessageRequest
{
    QueueUrl = "https://sqs.ap-southeast-1.amazonaws.com/123/snaptics-main-queue",
    MessageBody = JsonSerializer.Serialize(new { 
        InvoiceId = invoice.Id, 
        S3FilePath = s3Key, 
        Type = "OCR_SCAN_REQUIRED" 
    })
};
await _sqsClient.SendMessageAsync(requestMessage);
```
Tốc độ phản hồi của API lúc này gần như là `0.01 giây`!
