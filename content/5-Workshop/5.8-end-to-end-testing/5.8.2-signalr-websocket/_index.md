---
title: "WebSocket Testing (SignalR)"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.8.2. </b> "
---


Snaptics' "Real-time Notification" feature requires a continuous 2-way connection (WebSocket). AWS ALB supports WebSockets perfectly by default without any additional configuration.

### Testing Steps:

**1. Connect Client**
- Open another browser tab (Or use a Postman WebSocket Client).
- Connect to the SignalR Hub path on your server:
  `ws://<ALB_DNS_NAME>/hubs/notification?access_token=<TOKEN>`
- If the connection reports Status `101 Switching Protocols`, it means the WebSocket connection is successful!

**2. Send event from Server**
- Go back to the Swagger interface, call the Endpoint `POST /api/Transactions` to manually create a transaction (e.g., Spending 50,000 VND on breakfast).
- Immediately switch to the open WebSocket Client tab.
- You will see the Server proactively push a JSON object to the Client with the content:
  `{"type": "NEW_TRANSACTION_ADDED", "amount": 50000, "message": "New transaction has been saved!"}`

This proves the ALB is functioning correctly, accurately routing both the HTTP stream and the WebSocket stream into the `ECS Fargate` containers. The architecture is designed for scalability via ECS Service Auto Scaling.
