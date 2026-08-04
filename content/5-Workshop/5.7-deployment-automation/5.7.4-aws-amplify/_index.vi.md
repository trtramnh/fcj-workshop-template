---
title: "Triển khai Frontend bằng AWS Amplify"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.7.4. </b> "
---


Đối với giao diện người dùng (Angular app), Snaptics sử dụng **AWS Amplify** để hosting và triển khai cực kỳ dễ dàng. Amplify được kết nối trực tiếp với GitHub Repository của dự án, đồng nghĩa với việc mọi commit đẩy lên nhánh `main` đều tự động kích hoạt tiến trình Build & Deploy giao diện mới nhất.

<!-- TODO: Chèn ảnh chụp giao diện AWS Amplify Console (danh sách App) vào đây -->
![AWS Amplify Console](/images/5-Workshop/placeholder-amplify.png)

### CI/CD Workflow cho Frontend

Khác với Backend (phải tự định nghĩa file `deploy.yml`), AWS Amplify quản lý CI/CD một cách ngầm định:

1. **Kết nối Source Code:** Trên AWS Amplify Console, chúng ta kết nối với tài khoản GitHub và chọn repo `Snaptics-Client`.
2. **Cấu hình Build:** Amplify tự động nhận diện đây là ứng dụng Angular và cung cấp môi trường Node.js chuẩn. Nó tự động chạy `npm install` và `npm run build`.
3. **Continuous Deployment (Triển khai liên tục):** Mỗi khi có code mới hợp nhất vào nhánh `main`, Amplify tự động kéo code về, Build và Deploy lên mạng lưới CDN toàn cầu của AWS.

### File cấu hình Build (amplify.yml)

Amplify sử dụng tệp cấu hình Build Spec. Đằng sau hậu trường, nó thực hiện các dòng lệnh sau để biên dịch ứng dụng Angular:

```yaml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - npm install
    build:
      commands:
        - npm run build -- --configuration production
  artifacts:
    baseDirectory: dist/client
    files:
      - '**/*'
  cache:
    paths:
      - node_modules/**/*
```

### Phân phối toàn cầu & Chứng chỉ SSL
Với AWS Amplify, giao diện Web được tự động phân phối qua hệ thống mạng biên **Amazon CloudFront**, đảm bảo tốc độ tải trang siêu tốc bất kể người dùng ở đâu. Thêm vào đó, Amplify cũng tự động cấu hình và gia hạn chứng chỉ bảo mật SSL/TLS hoàn toàn miễn phí!
