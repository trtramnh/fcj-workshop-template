---
title: "SQS Queues & SNS Topics"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---



<!-- TODO: Insert screenshots of Amazon SQS queue (snaptics-ai-queue) and Amazon SNS topic here -->
![SQS Queue](/images/5-Workshop/placeholder-sqs.png)
![SNS Topic](/images/5-Workshop/placeholder-sns.png)

AWS SQS (Simple Queue Service) and SNS (Simple Notification Service) combine to form the backbone for Snaptics' Event-Driven architecture.

### 1. Create SQS Queue
- Open **Amazon SQS ➔ Create queue**.
- **Type:** Standard.
- **Name:** `snaptics-main-queue`
- **Visibility timeout:** `1 minutes` (Time to hide the message so other workers don't process it simultaneously).
- Click **Create queue**. Copy the **URL** (e.g., `https://sqs.ap-southeast-1.amazonaws.com/123/snaptics-main-queue`).

### 2. Create SNS Topic
SNS is used to send Push Notifications (or Email alerts to Admins) instantly to multiple different subscribers.
- Open **Amazon SNS ➔ Topics ➔ Create topic**.
- **Type:** Standard.
- **Name:** `snaptics-alerts`
- Click **Create topic**. 
- Go to the newly created Topic, click **Create subscription**, choose Protocol as `Email`, enter your Email address, then open your inbox and click Confirm. From now on, system errors will be alerted directly to your Email.

### Integration in Snaptics (.NET C#)
The Snaptics application uses the `AWSSDK.SQS` package and injects the `IAmazonSQS` interface via Dependency Injection (DI) in `Program.cs`. 

When there is a new User invoice upload, instead of directly calling the OCR function which takes 5-10 seconds, the API simply throws a message into SQS:

```csharp
// In the Invoice Controller
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
The API response time is now almost `0.01 seconds`!
