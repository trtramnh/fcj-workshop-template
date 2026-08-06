---
title: "Tự động hóa Triển khai (CI/CD)"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---


Quên mấy cái script gõ bằng tay ở máy Local đi! Trong môi trường Enterprise thực thụ, Developer chỉ có một nhiệm vụ duy nhất là viết code và Commit lên GitHub. Mọi thao tác Build, Test, và Deploy cho cả Backend (.NET) và Frontend (Angular) sẽ do **GitHub Actions** tự động gánh vác 100%.

## 1. Thiết lập GitHub Secrets 

GitHub Actions cần quyền vào tài khoản AWS của bạn để đẩy Docker Image và gọi lệnh Deploy.
1. Mở repository Snaptics của bạn trên GitHub.
2. Vào tab **Settings ➔ Secrets and variables ➔ Actions**.
3. Bấm **New repository secret**. Nhập lần lượt 3 biến sau:
   - `AWS_ACCESS_KEY_ID`: Dán cái Access Key ID của user `github-actions-snaptics` lúc nãy.
   - `AWS_SECRET_ACCESS_KEY`: Dán cái Secret Key vào.
   - `AWS_REGION`: Gõ `ap-southeast-1`

## 2. Pipeline Triển khai Backend (ECS Fargate)

Trong thư mục code của bạn, hãy tạo một file tại đường dẫn `.github/workflows/deploy.yml` với nội dung như sau:

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
        echo "Building docker image..."
        docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
        echo "Pushing image to ECR..."
        docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG

    - name: Force New Deployment in Fargate
      run: |
        echo "Telling ECS to fetch the new image..."
        aws ecs update-service --cluster ${{ env.ECS_CLUSTER }} --service ${{ env.ECS_SERVICE }} --force-new-deployment --region ${{ env.AWS_REGION }}
```

### Cơ chế Zero-Downtime thần thánh
Dòng lệnh cuối cùng `--force-new-deployment` kích hoạt cơ chế **Rolling Update** cực kỳ thông minh của AWS ECS. Load Balancer (ALB) vẫn sẽ cho người dùng xài phiên bản cũ bình thường. Trong lúc đó, nó âm thầm lôi Image mới về, khởi động các Container mới. Chỉ khi nào Container mới khỏe 100% (Trả về HTTP 200), ALB mới chớp nhoáng "bẻ ghi" toàn bộ traffic sang nhà mới và nhẹ nhàng tiêu diệt các Container cũ. Quá trình nâng cấp diễn ra mượt mà mà người dùng không hề hay biết!

## 3. Pipeline Triển khai Frontend (AWS Amplify)

Việc Hosting một trang Angular SPA lên AWS cực kỳ dễ dàng nhờ **AWS Amplify**. Bạn thậm chí không cần viết file YAML lằng nhằng, Amplify có sẵn cổng giao tiếp với GitHub.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.7-deployment-automation/amplify_create_1.jpg" >
  </div>
    <div> <img src="/fcj-workshop-template/images/5-Workshop/5.7-deployment-automation/amplify_create_1.2.jpg" >
  </div>
    <div> <img src="/fcj-workshop-template/images/5-Workshop/5.7-deployment-automation/amplify_create_1.3.jpg" >
  </div>
    <div> <img src="/fcj-workshop-template/images/5-Workshop/5.7-deployment-automation/amplify_create_1.4.jpg" >
  </div>

### Thiết lập Amplify Auto-Deploy
1. Vào AWS Console, mở dịch vụ **AWS Amplify**.
2. Bấm **Create new app** ➔ Chọn nguồn là **GitHub**.
3. Cấp quyền đăng nhập GitHub và trỏ vào repository `Snaptics` của bạn.
4. Trỏ vào thư mục `Frontend` (nơi chứa code Angular).
5. Amplify sẽ tự động nhận diện lệnh build chuẩn (ví dụ `npm run build`).
6. Bấm **Save and Deploy**.

Xong! Kể từ giây phút này, mỗi khi Frontend Developer gõ lệnh `git push`, AWS Amplify sẽ lập tức bắt được tín hiệu (Webhook), tự động kéo code về, Build ra các file tĩnh (HTML/JS/CSS) và ném toàn bộ lên hạ tầng Edge CDN cực nhanh của nó. Không cần động tay động chân thêm một lần nào nữa!