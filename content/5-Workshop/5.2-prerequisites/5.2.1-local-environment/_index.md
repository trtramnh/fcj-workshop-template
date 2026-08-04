---
title: "Local Environment"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---


You need to install the following tools on your computer.

### 1. Install AWS CLI v2
This is a mandatory tool to interact with AWS resources from the terminal, especially when running the `deploy.ps1` script.
- Download from the homepage: [AWS CLI Version 2](https://aws.amazon.com/cli/)
- After installation, open Terminal/PowerShell and configure security credentials:

```bash
aws configure
```
The system will prompt you for 4 parameters:
- **AWS Access Key ID:** `<YOUR_ACCESS_KEY_ID>` (Get this from your IAM User)
- **AWS Secret Access Key:** `<YOUR_SECRET_ACCESS_KEY>`
- **Default region name:** `ap-southeast-1` (Choose an appropriate region to get the lowest latency)
- **Default output format:** `json`

### 2. Install Docker
Snaptics runs on a Serverless Container architecture (ECS Fargate), so you must package the application into a Docker Image before pushing it to AWS.
- Download **Docker Desktop** (for Windows/Mac) at [docker.com](https://www.docker.com/products/docker-desktop/).
- Verify if Docker is running successfully:
```bash
docker info
```

### 3. Install .NET 8 / 9 SDK
Although you can build inside Docker, having the SDK on your machine helps you run Entity Framework Migrations, test the app locally, or configure `appsettings.json` files.
- Download at [dotnet.microsoft.com](https://dotnet.microsoft.com/download).

### 4. Prepare AI API Keys
Since Snaptics relies on AI to extract invoices, ensure you have created accounts and obtained these 2 API keys:
1. **Google Gemini API Key**: Get it at [Google AI Studio](https://aistudio.google.com/app/apikey).
2. **Azure Document Intelligence**: Log in to [Azure Portal](https://portal.azure.com/), create a Document Intelligence resource, and copy the **Endpoint** and **Key 1**.
