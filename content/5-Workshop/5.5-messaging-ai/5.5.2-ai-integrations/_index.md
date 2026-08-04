---
title: "AI Integrations"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---


Snaptics communicates with 2 leading AI models via REST API (Keys are stored in SSM Parameter Store).

### 1. OCR Analysis with Azure Document Intelligence

Azure is renowned for its Pre-built Receipt Model. Below is how Snaptics (inside the .NET Service) calls Azure to scan the invoice image located on S3:

```csharp
// Using Azure.AI.FormRecognizer SDK
var credential = new AzureKeyCredential(_config["AiSettings:AzureDocIntelKey"]);
var client = new DocumentAnalysisClient(new Uri(_config["AiSettings:AzureDocIntelEndpoint"]), credential);

// Generate Pre-signed URL from S3 image file
string preSignedUrl = _s3Service.GeneratePreSignedUrl("s3-bucket-snaptics", s3Key, 10);

// Analyze with 'prebuilt-receipt' model
AnalyzeDocumentOperation operation = await client.AnalyzeDocumentFromUriAsync(WaitUntil.Completed, "prebuilt-receipt", new Uri(preSignedUrl));
var result = operation.Value;

// Automatically extract Merchant Name and Total Amount
string merchantName = result.Documents[0].Fields["MerchantName"].Value.AsString();
double total = result.Documents[0].Fields["Total"].Value.AsDouble();
```

### 2. Financial Consulting with Google Gemini

Google Gemini has incredibly fast processing speed (especially the 1.5 Flash model). Snaptics has a feature to calculate the end-of-month balance and ask Gemini for comments:

```csharp
var prompt = $"I spent {totalExpense} VND this month on Entertainment and Dining. " +
             $"My budget is {budget} VND. Give me a brief advice in Vietnamese.";

// Call the Gemini API Endpoint
var requestBody = new {
    contents = new[] { new { parts = new[] { new { text = prompt } } } }
};

var response = await _httpClient.PostAsJsonAsync(
    $"https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key={_config["AiSettings:GeminiApiKey"]}", 
    requestBody);
```
With the VPC design featuring a `NAT Gateway`, these outbound HTTP calls to Azure and Google work smoothly and securely.
