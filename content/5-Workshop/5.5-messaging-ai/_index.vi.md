---
title: "AI & Tác vụ nền (Messaging)"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---


Trái tim sức mạnh của Snaptics nằm ở khả năng xử lý bất đồng bộ (Asynchronous processing). Việc người dùng tải lên một hóa đơn đòi hỏi hệ thống phải quét OCR, xử lý phân loại bằng AI và lưu trữ. Thay vì bắt người dùng chờ xoay vòng vòng trên UI, Snaptics sẽ quăng (publish) tác vụ đó vào một hàng đợi (Queue), báo cho người dùng biết "Đang xử lý", và thực thi ngầm nó thông qua Hangfire và SQS.
