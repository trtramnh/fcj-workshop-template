---
title: "Quản lý Secret (SSM)"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---


Để ứng dụng .NET kết nối được tới RDS và gọi được AI APIs, nó cần các khóa bí mật. Thay vì đẩy file `appsettings.Production.json` lên Git và làm lộ mật khẩu, chúng ta sẽ lưu chúng lên Parameter Store. Khi Container khởi động, .NET sẽ tự động gọi AWS SDK lấy cấu hình về.

### Tạo Parameter
Vào **AWS Systems Manager ➔ Parameter Store ➔ Create parameter**.
Tạo lần lượt các biến sau, luôn chọn `Type: SecureString` để chúng được mã hóa bằng KMS:

1. **Chuỗi kết nối SQL Server:**
   - **Name:** `/Snaptics/Production/ConnectionStrings:DefaultConnection`
   - **Value:** `Server=<YOUR_RDS_ENDPOINT>,1433;Database=SnapticsDB;User Id=admin;Password=Snaptics@StrongPass123!;TrustServerCertificate=True;`

2. **JWT Secret Key:**
   - **Name:** `/Snaptics/Production/TokenKey`
   - **Value:** Nhập một chuỗi ngẫu nhiên dài 64 ký tự.

3. **Google Gemini API:**
   - **Name:** `/Snaptics/Production/AiSettings:GeminiApiKey`
   - **Value:** Nhập API Key Google của bạn.

4. **Azure Document Intelligence:**
   - **Name:** `/Snaptics/Production/AiSettings:AzureDocIntelKey`
   - **Value:** Nhập Key của Azure.

### Nó hoạt động trong code .NET thế nào?

Bên trong ứng dụng Snaptics (`Program.cs`), tính năng `AddSystemsManager` tự động tìm nạp mọi tham số có tiền tố (prefix) trùng khớp:

```csharp
// Program.cs - Tự động nạp cấu hình từ AWS
builder.Configuration.AddSystemsManager(configureSource =>
{
    configureSource.Path = "/Snaptics/Production/";
    configureSource.ReloadAfter = TimeSpan.FromMinutes(5); // Tự reload cấu hình nếu trên AWS có thay đổi
});
```
Nhờ dấu `:` trong tên Parameter, biến `AiSettings:GeminiApiKey` tự động khớp hoàn hảo với model class `AiSettings` trong ứng dụng của bạn.
