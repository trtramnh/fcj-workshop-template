---
title: "GitHub Actions CI/CD"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.7.3. </b> "
---


Mặc dù script `deploy.ps1` rất tiện lợi cho việc triển khai thủ công từ máy cá nhân, nhưng môi trường Production thực tế đòi hỏi một hệ thống tự động hoàn toàn (CI/CD). Snaptics sử dụng **GitHub Actions** để tự động hóa quá trình Build & Deploy ngay khi có thay đổi mã nguồn.

<!-- TODO: Chèn ảnh chụp giao diện GitHub Actions (Run thành công) vào đây -->
![GitHub Actions Run](/images/5-Workshop/placeholder-actions.png)

### Cấu hình Workflow

Mỗi khi có người push code lên nhánh `main` hoặc `master`, GitHub Actions sẽ tự động kích hoạt tiến trình triển khai. File cấu hình này nằm ở `.github/workflows/deploy.yml` trong repository Backend:

```yaml
name: Deploy Backend to Fargate

on:
  push:
    branches:
      - main
      - master

env:
  AWS_REGION: ap-southeast-1
  ECR_REPOSITORY: snaptics-api
  ECS_SERVICE: snaptics-backend-service
  ECS_CLUSTER: Snaptics-Cluster

jobs:
  deploy:
    name: Build and Deploy
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v4

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ env.AWS_REGION }}

    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v2

    - name: Build, tag, and push image to ECR
      id: build-image
      env:
        ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
        IMAGE_TAG: latest
      run: |
        docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG

    - name: Force New Deployment in Fargate
      run: |
        aws ecs update-service --cluster ${{ env.ECS_CLUSTER }} --service ${{ env.ECS_SERVICE }} --force-new-deployment --region ${{ env.AWS_REGION }}
```

### Vấn đề bảo mật
Lưu ý rằng chúng ta bảo mật thông tin đăng nhập AWS (`AWS_ACCESS_KEY_ID` và `AWS_SECRET_ACCESS_KEY`) bằng cách sử dụng **GitHub Repository Secrets**. Những key này tuyệt đối không được gán cứng (hardcode) trong code, giúp ngăn chặn rủi ro lộ lọt dữ liệu đám mây.
