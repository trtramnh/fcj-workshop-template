---
title: "Secrets Management (SSM)"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---


For the .NET application to connect to RDS and call AI APIs, it needs secret keys. Instead of pushing the `appsettings.Production.json` file to Git and exposing passwords, we will store them in Parameter Store. When the Container starts, .NET will automatically invoke the AWS SDK to fetch configurations.

### Create Parameter
Go to **AWS Systems Manager ➔ Parameter Store ➔ Create parameter**.
Create the following variables one by one, always choosing `Type: SecureString` so they are encrypted with KMS:

1. **SQL Server Connection String:**
   - **Name:** `/Snaptics/Production/ConnectionStrings:DefaultConnection`
   - **Value:** `Server=<YOUR_RDS_ENDPOINT>,1433;Database=SnapticsDB;User Id=admin;Password=Snaptics@StrongPass123!;TrustServerCertificate=True;`

2. **JWT Secret Key:**
   - **Name:** `/Snaptics/Production/TokenKey`
   - **Value:** Enter a random string of 64 characters.

3. **Google Gemini API:**
   - **Name:** `/Snaptics/Production/AiSettings:GeminiApiKey`
   - **Value:** Enter your Google API Key.

4. **Azure Document Intelligence:**
   - **Name:** `/Snaptics/Production/AiSettings:AzureDocIntelKey`
   - **Value:** Enter your Azure Key.

### How does it work in .NET code?

Inside the Snaptics application (`Program.cs`), the `AddSystemsManager` feature automatically fetches all parameters with matching prefixes:

```csharp
// Program.cs - Auto-load configs from AWS
builder.Configuration.AddSystemsManager(configureSource =>
{
    configureSource.Path = "/Snaptics/Production/";
    configureSource.ReloadAfter = TimeSpan.FromMinutes(5); // Auto-reload config if changed on AWS
});
```
Thanks to the `:` in the Parameter name, the variable `AiSettings:GeminiApiKey` perfectly and automatically maps to the `AiSettings` model class in your application.
