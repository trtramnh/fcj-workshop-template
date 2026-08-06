---
title: "Messaging & AI"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---


The core strength of Snaptics lies in its asynchronous processing capabilities. By utilizing Amazon SQS and AI services, Snaptics provides a seamless user experience even during heavy analytical tasks.

## 1. Amazon SQS and Dead Letter Queue (DLQ)

In a production environment, simply having a queue is not enough. What happens if the AI API is temporarily down and an invoice fails to process? The message would bounce back into the queue and loop endlessly. To prevent this, we implement a **Dead Letter Queue (DLQ)**.

### A. Create the DLQ (For failed messages)
- Open **Amazon SQS ➔ Create queue**.
- **Type:** Standard.
- **Name:** `snaptics-ai-queue-dlq` (Notice the `-dlq` suffix).
- Keep default settings and click Create.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.5-messaging-ai/amazon_sqs_create.jpg" >
  </div>

### B. Create the Main Queue
- Go back and **Create queue**.
- **Type:** Standard.
- **Name:** `snaptics-ai-queue` (Must match the architecture diagram).
- **Visibility timeout:** `1 minutes`.
- **Dead-letter queue:** Expand this section, check **Enabled**.
- Select the `snaptics-ai-queue-dlq` you just created.
- **Maximum receives:** `3`. *(This means if the ECS container fails to process an invoice 3 times, the message is automatically banished to the DLQ to stop the looping failure).*
- Click Create.

## 2. Artificial Intelligence (AI) Implementation

The `.NET` container pulls messages from `snaptics-ai-queue` and calls external AI APIs.

### Azure Document Intelligence (OCR)
When a user uploads a receipt, AWS routes it to Azure for text extraction:
```csharp
// Fetch the secure key injected from AWS Systems Manager via IConfiguration
var azureKey = _config["AiSettings:AzureDocIntelKey"];
var azureEndpoint = _config["AiSettings:AzureDocIntelEndpoint"];
var credential = new AzureKeyCredential(azureKey);
var client = new DocumentAnalysisClient(new Uri(azureEndpoint), credential);

// Analyze the S3 Image via the Gateway Endpoint pre-signed URL
AnalyzeDocumentOperation operation = await client.AnalyzeDocumentFromUriAsync(WaitUntil.Completed, "prebuilt-receipt", new Uri(preSignedUrl));
var result = operation.Value;

// Extract Merchant and Total
string merchantName = result.Documents[0].Fields["MerchantName"].Value.AsString();
double total = result.Documents[0].Fields["Total"].Value.AsDouble();
```

### Google Gemini (Lightning-fast OCR & Consulting)
Snaptics leverages the latest generation Gemini model (Gemini 2.5 Flash) for super-fast invoice extraction and smart financial advice:
```csharp
var requestBody = new {
    contents = new[] { new { parts = new[] { new { text = userPrompt } } } }
};

var apiKey = _config["AiSettings:GeminiApiKey"];
var modelName = _config["AiSettings:GeminiModel"]; // e.g. gemini-2.5-flash
var endpoint = $"https://generativelanguage.googleapis.com/v1beta/models/{modelName}:generateContent?key={apiKey}";

var response = await _httpClient.PostAsJsonAsync(endpoint, requestBody);
```

## 3. Background Jobs with Hangfire

For scheduled tasks (like rolling over budget limits at midnight), Snaptics uses Hangfire running concurrently inside the Fargate containers.

```csharp
// Hangfire uses the same SQL Server Database for job storage
builder.Services.AddHangfire(config => config
    .UseSqlServerStorage(builder.Configuration.GetConnectionString("DefaultConnection")));
builder.Services.AddHangfireServer();

// Automatically send financial reports on the 1st of every month
RecurringJob.AddOrUpdate<IReportService>(
    "monthly-ai-insight",
    job => job.GenerateMonthlyInsightsAsync(),
    "0 0 1 * *"); // Cron expression
```

By combining SQS for event-driven tasks and Hangfire for cron jobs, the Snaptics backend remains incredibly responsive!