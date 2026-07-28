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
- **Elastic Beanstalk** quản lý application environment, còn FastAPI chạy trên EC2 instance của environment.
- **S3** lưu thumbnail, video và tài liệu khóa học ở chế độ private.
- **CloudWatch** thu thập application log từ Elastic Beanstalk và các operational metric.
- **Systems Manager Parameter Store** lưu secret backend dưới dạng mã hóa.
- **Supabase PostgreSQL** lưu dữ liệu ứng dụng qua kết nối TLS.

Elastic Beanstalk được cấu hình Single instance để kiểm soát chi phí trong phạm
vi bài thực tập. Đây là thiết kế thực tế cho demo, chưa phải thiết kế production
multi-AZ có high availability đầy đủ.

EduCloud chỉ cung cấp khóa học miễn phí. Pricing, checkout và payment service được chủ động loại khỏi kiến trúc.

[Tải file kiến trúc có thể chỉnh sửa](/files/educloud-aws-architecture.drawio)
