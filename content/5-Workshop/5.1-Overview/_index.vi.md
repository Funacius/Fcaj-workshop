---
title: "Giới thiệu"
weight: 1
chapter: false
pre: "<b>5.1.</b>"
---

# Giới thiệu

Phần này giải thích kiến trúc triển khai của EduCloud Lite trước khi bắt đầu tạo
tài nguyên AWS. Nắm phần này trước sẽ giúp các bước sau liên kết với nhau thay
vì chỉ là những thao tác rời rạc trên console.

## Nội dung

1. [Kiến trúc hệ thống](5.1.1-architecture/)
2. [Luồng request](5.1.2-request-flow/)

Kiến trúc được thiết kế để dùng số lượng dịch vụ vừa đủ cho bài nộp, nhưng vẫn
có xác thực managed, secret mã hóa, S3 private, HTTPS delivery và CI/CD.
