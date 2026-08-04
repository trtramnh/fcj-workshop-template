---
title: "Deploy Frontend to AWS Amplify"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.7.4. </b> "
---


For the user-facing Client (Angular application), Snaptics utilizes **AWS Amplify** for seamless deployment and hosting. Amplify connects directly to our GitHub repository, meaning any push to the `main` branch automatically triggers a new frontend build and deployment.

<!-- TODO: Insert screenshot of AWS Amplify Console showing your app here -->
![AWS Amplify Console](/images/5-Workshop/placeholder-amplify.png)

### CI/CD Workflow for Frontend

Unlike the backend which requires an explicit `deploy.yml` workflow, AWS Amplify handles the CI/CD pipeline implicitly:

1. **Connect Repository:** In the AWS Amplify console, we connect our GitHub repository hosting the `Snaptics-Client`.
2. **Build Settings:** Amplify detects that it's an Angular app and provisions the correct build environment (Node.js). It runs `npm install` and `npm run build`.
3. **Continuous Deployment:** Every time code is merged into `main`, Amplify automatically builds the new version and deploys it to the global CloudFront CDN.

### Build Specifications (amplify.yml)

Amplify uses a build specification configuration. Behind the scenes, it executes a script similar to this to compile the Angular code:

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

### Global Distribution & SSL
By using AWS Amplify, the frontend is automatically hosted across the **Amazon CloudFront** Edge Network, ensuring lightning-fast load times globally. Furthermore, Amplify provisions and auto-renews free SSL/TLS certificates, ensuring end-to-end encryption for all our users.
