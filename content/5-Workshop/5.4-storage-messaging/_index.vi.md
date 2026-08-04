---
title: "Database, Storage & Secrets"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---


Ở phân hệ này, chúng ta sẽ thiết lập lớp Lưu trữ Dữ liệu (Data Storage Layer). Snaptics có nhu cầu lưu trữ hỗn hợp:
- Dữ liệu có cấu trúc (Giao dịch, User, Hạng mục chi tiêu) sẽ nằm trong **SQL Server**.
- Dữ liệu phi cấu trúc (Hình ảnh hóa đơn, biên lai) sẽ lưu ở **S3**.
- Dữ liệu nhạy cảm (Mật khẩu DB, JWT Secret, API Keys) sẽ được mã hóa tại **Parameter Store**.
