---
title: "Kiểm thử Hệ thống (E2E Testing)"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---


Khi mọi thành phần từ Network, Database, ECS đến AI đã khởi chạy thành công. Chúng ta tiến hành kiểm thử End-to-End (E2E) để đảm bảo luồng dữ liệu đi đúng thiết kế.

Sẽ có 2 bước kiểm thử chính:
1. Giao tiếp qua REST API (Dùng Swagger UI).
2. Giao tiếp qua WebSocket (Dùng SignalR).
