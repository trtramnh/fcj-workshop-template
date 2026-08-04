---
title: "Dockerizing .NET API"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---


<!-- TODO: ChÃ¨n áº£nh mÃ n hÃ¬nh Amazon ECR chá»©a Docker image cá»§a Backend vÃ o Ä‘Ã¢y -->
![Amazon ECR](/images/5-Workshop/placeholder-ecr.png)
Bước đầu tiên để đưa ứng dụng lên Fargate là phải đóng gói nó thành một Docker Container. Dưới đây là nội dung chuẩn của file `Dockerfile` cho ứng dụng .NET 8 (thường nằm ở thư mục root).

```dockerfile
# Sử dụng image base của ASP.NET Core cho môi trường chạy
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

# Sử dụng image chứa SDK để build ứng dụng
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy các file dự án (.csproj) và phục hồi thư viện (NuGet packages)
COPY ["API/API.csproj", "API/"]
COPY ["BLL/BLL.csproj", "BLL/"]
COPY ["DAL/DAL.csproj", "DAL/"]
RUN dotnet restore "API/API.csproj"

# Copy toàn bộ code còn lại và tiến hành build
COPY . .
WORKDIR "/src/API"
RUN dotnet build "API.csproj" -c Release -o /app/build

# Public ứng dụng (Tạo file thực thi)
FROM build AS publish
RUN dotnet publish "API.csproj" -c Release -o /app/publish

# Gộp vào image cuối cùng (Cực kỳ nhẹ)
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "API.dll"]
```

### Đẩy Image lên Amazon ECR
Tiếp theo, mở **Amazon ECR ➔ Repositories ➔ Create repository**. Tạo một kho chứa Private tên là `snaptics-api`.

Mở terminal ở máy tính của bạn và chạy chùm lệnh sau để build và đẩy (Push) image lên AWS:
```bash
# 1. Đăng nhập Docker vào hệ thống AWS (Thay <ACCOUNT_ID> bằng ID của bạn)
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com

# 2. Build Docker image
docker build -t snaptics-api .

# 3. Gắn tag cho image
docker tag snaptics-api:latest <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/snaptics-api:latest

# 4. Push lên Đám mây
docker push <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/snaptics-api:latest
```
Đến đây, ứng dụng của bạn đã nằm an toàn trên AWS ECR.
