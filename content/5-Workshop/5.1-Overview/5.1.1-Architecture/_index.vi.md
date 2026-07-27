---
title: "Kiến trúc hệ thống"
weight: 1
chapter: false
pre: "<b>5.1.1.</b>"
---

# Kiến trúc hệ thống

![Kiến trúc AWS EduCloud](/images/educloud-aws-architecture.png)

EduCloud Lite sử dụng một kiến trúc AWS gọn để phục vụ bài nộp:

- **Amplify Hosting** host React/Vite single-page application.
- **Amazon Cognito** xử lý đăng nhập và xác thực người dùng.
- **CloudFront** cung cấp một HTTPS entry point cho API và media private.
- **Elastic Beanstalk** chạy FastAPI backend.
- **S3** lưu thumbnail, video và tài liệu khóa học ở chế độ private.
- **Systems Manager Parameter Store** lưu secret backend dưới dạng mã hóa.
- **Supabase PostgreSQL** lưu dữ liệu ứng dụng qua kết nối TLS.

Elastic Beanstalk được cấu hình Single instance để kiểm soát chi phí trong phạm
vi bài thực tập. Đây là thiết kế thực tế cho demo, chưa phải thiết kế production
multi-AZ có high availability đầy đủ.

[Tải file kiến trúc có thể chỉnh sửa](/files/educloud-aws-architecture.drawio)

