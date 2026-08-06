---
title: "E2E Testing"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---


Congratulations on deploying the Enterprise architecture! Now it's time to verify that all components (CloudFront ➔ ALB ➔ ECS ➔ SQL Server/S3/SQS) are communicating perfectly.

## 1. CloudFront Verification

Instead of hitting the ALB directly, open your browser and navigate to your **CloudFront Domain** (e.g., `https://d1234abcd.cloudfront.net/swagger` or your Custom Route 53 Domain).

1. **Test Normal Traffic:** Access the normal Swagger UI. It should load exceptionally fast because CloudFront optimizes the delivery through its edge locations.

  <div> <img src="/fcj-workshop-template/images/5-Workshop/5.8-end-to-end-testing/cloudfront.jpg" >
  </div>

## 2. API & AI Integration Testing (Swagger)

1. **Auth:** Call `/api/Auth/register` then `/api/Auth/login`. This proves the ECS container can successfully connect to the **SQL Server Database** to verify credentials using the password fetched from **Parameter Store**.
2. **Upload Invoice:** Call `/api/Transaction/from-bill` and upload an image file.
3. **Verify Pipeline:**
   - Go to **S3**, you should see the image. (Proving the **VPC Gateway Endpoint** works).
   - Go to **SQS**, you should see a message briefly appear in `snaptics-ai-queue`.
   - Go to **CloudWatch Logs**, you should see the Hangfire worker picking up the message and calling Azure/Gemini AI.
   - Finally, Swagger should return the accurately extracted merchant name and total price!

## 3. WebSocket Testing (SignalR)

Real-time notifications must survive the journey through CloudFront and ALB.

1. Connect your WebSocket client to: `wss://<CLOUDFRONT_DOMAIN>/hubs/notification?access_token=<TOKEN>`
2. Ensure you receive a `101 Switching Protocols` status.
3. Keep the connection open. Go back to Swagger and manually add a new spending transaction.
4. Your WebSocket client should instantly receive a JSON payload: `{"type": "NEW_TRANSACTION_ADDED"}`. 

This confirms that CloudFront and the ALB are correctly upgrading HTTP connections to WebSockets and maintaining persistent tunnels to the Fargate containers!