---
title: "Script Triển khai (deploy.ps1)"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---


Snaptics sở hữu một kịch bản PowerShell tự động tên là `deploy.ps1` nằm ở thư mục gốc. Mỗi khi bạn hoàn tất một tính năng và muốn đưa lên hệ thống thật, bạn chỉ cần gõ duy nhất một lệnh:

```powershell
.\deploy.ps1
```

### Nội dung và giải thích Script

Dưới đây là nguyên văn đoạn script và công dụng của từng khối lệnh:

```powershell
# [1/5] Định nghĩa các biến môi trường
$ECR_REGISTRY = "<ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com"
$IMAGE_NAME = "snaptics-api"
$REGION = "ap-southeast-1"
$CLUSTER_NAME = "Snaptics-Cluster"
$SERVICE_NAME = "snaptics-backend-service"

Write-Host "=========================================="
Write-Host "🚀 BẮT ĐẦU QUÁ TRÌNH DEPLOY SNAPCTICS-API"
Write-Host "=========================================="

# [2/5] Đăng nhập vào AWS ECR thông qua token bảo mật
Write-Host "[1/4] Dang dang nhap vao AWS ECR..."
aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin $ECR_REGISTRY

# [3/5] Gọi lệnh build file Dockerfile
Write-Host "[2/4] Dang build Docker image moi nhat..."
docker build -t ${IMAGE_NAME}:latest .

# [4/5] Gắn nhãn và Đẩy Image lên Đám mây
Write-Host "[3/4] Dang day image len AWS ECR..."
docker tag ${IMAGE_NAME}:latest ${ECR_REGISTRY}/${IMAGE_NAME}:latest
docker push ${ECR_REGISTRY}/${IMAGE_NAME}:latest

# [5/5] Cực kỳ quan trọng: Lệnh ép ECS tải lại code mới
Write-Host "[4/4] Ra lenh ECS cap nhat Service (Force New Deployment)..."
aws ecr update-service --cluster $CLUSTER_NAME --service $SERVICE_NAME --force-new-deployment --region $REGION

Write-Host "=========================================="
Write-Host "🎉 DA GUI LENH DEPLOY THANH CONG!"
Write-Host "AWS dang cap nhat ngam. Vui long doi 2-3 phut."
Write-Host "=========================================="
```
Nhờ có script này, thời gian release tính năng mới (Time-to-market) giảm từ 10 phút xuống chỉ còn 30 giây!
