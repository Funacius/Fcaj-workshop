---
title: "Secret và database"
weight: 3
chapter: false
pre: "<b>5.3.</b>"
---

# Cấu hình secret và database

EduCloud Lite cần PostgreSQL để lưu users, courses, lessons, learning progress,
assessments và certificates. Backend đọc database URL và JWT secret từ AWS
Systems Manager Parameter Store thay vì lưu secret trực tiếp trong source code.

## Nội dung

1. [Vì sao chọn Supabase](5.3.1-why-supabase/)
2. [Tạo Supabase connection](5.3.2-supabase-connection/)
3. [Lưu secret vào Parameter Store](5.3.3-parameter-store/)
4. [Cấp quyền IAM](5.3.4-iam-permission/)

