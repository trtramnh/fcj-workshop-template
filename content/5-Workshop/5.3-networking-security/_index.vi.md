---
title: "Mạng & Bảo mật"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---


Để cô lập Snaptics khỏi các mỗi đe dọa từ Public Internet, chúng ta sẽ xây dựng một kiến trúc Virtual Private Cloud (VPC) theo chuẩn **Multi-Tier**. 

Nguyên tắc vàng: Tất cả tài nguyên xử lý dữ liệu (như Container của ECS và Database của RDS) bắt buộc phải nằm trong các Private Subnet, không có địa chỉ IP Public. Chúng chỉ có thể được truy cập thông qua một Application Load Balancer nằm ở Public Subnet.
