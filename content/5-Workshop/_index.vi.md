---
title: "Workshop"
weight: 5
chapter: true
pre: "<b>5.</b>"
---

# EduCloud Lite – Nền tảng học tập trên AWS

Chào mừng bạn đến với workshop kỹ thuật của **EduCloud Lite**, một nền tảng
quản lý học tập trên cloud được xây dựng bằng React, FastAPI, Amazon Cognito,
Amazon S3, CloudFront, AWS Amplify, Elastic Beanstalk và cơ sở dữ liệu
PostgreSQL được quản lý. Nội dung dưới đây giải thích các quyết định kiến trúc
và hướng dẫn triển khai lại ứng dụng trong AWS account của riêng bạn.

- **Website đang chạy:** [Mở EduCloud Lite](https://main.djk00b5qbck73.amplifyapp.com/courses)
- **Sơ đồ kiến trúc:** [Tải file draw.io có thể chỉnh sửa](../files/educloud-aws-architecture.drawio)
- **Mã nguồn:** [EduCloud trên GitHub](https://github.com/Funacius/EduCloud)
- **Hướng dẫn build và deploy:** [Tải tài liệu PDF](../files/EduCloud-Build-Deployment-Guide.pdf)

![Sơ đồ kiến trúc AWS của EduCloud Lite](/images/educloud-aws-architecture.png)

## Tổng quan

EduCloud Lite tách riêng giao diện trình duyệt, API ứng dụng, quản lý danh
tính, lưu trữ object và cơ sở dữ liệu. Frontend được build và host bằng
Amplify. FastAPI chạy trên Elastic Beanstalk. Cognito cấp token định danh cho
người dùng, còn Supabase PostgreSQL lưu dữ liệu ứng dụng. Các file khóa học
được upload vào S3 private và phân phối qua CloudFront với Origin Access
Control, vì vậy bucket không cần bật public access.

Workshop được thiết kế theo hướng thực hành: mỗi phần tương ứng với một công
việc triển khai thật, có các thiết lập AWS liên quan và giải thích lý do về bảo
mật hoặc chi phí. Hãy dùng AWS account và thông tin database của chính bạn;
không sao chép secret từ các ảnh minh họa.

## Mục lục tài liệu

Bạn có thể chọn trực tiếp một phần chính bên dưới:

1. [Giới thiệu](5.1-overview/) – kiến trúc và luồng request
   - [Kiến trúc](5.1-overview/5.1.1-architecture/)
   - [Luồng request](5.1-overview/5.1.2-request-flow/)
2. [Chuẩn bị](5.2-prerequisites/) – account, công cụ và mã nguồn
3. [Secret và database](5.3-secrets-and-database/) – Supabase, Parameter Store và IAM
4. [Xác thực với Cognito](5.4-authentication/) – user pool, app client và lần đăng nhập đầu tiên
5. [Triển khai backend](5.5-backend/) – đóng gói FastAPI và Elastic Beanstalk
6. [Lưu trữ private và CloudFront](5.6-storage-delivery/) – S3, OAC và đường dẫn phân phối
7. [Triển khai frontend](5.7-frontend/) – biến môi trường và SPA routing trên Amplify
8. [Kiểm tra và xử lý lỗi](5.8-validation/) – kiểm thử website thực tế
9. [Dọn dẹp](5.9-cleanup/) – xóa tài nguyên sau khi nộp bài
