---
title: "Workshop"
weight: 5
chapter: false
pre: "<b>5.</b>"
---

# Triển khai EduCloud Lite trên AWS

Workshop hướng dẫn người đọc triển khai một môi trường EduCloud Lite của riêng
họ. Mỗi người phải sử dụng AWS account, Cognito User Pool, S3 bucket, Parameter
Store và PostgreSQL database của chính mình.

## Các phần

1. [Giới thiệu](5.1-overview/)
2. [Chuẩn bị](5.2-prerequisites/)
3. [Secret và database](5.3-secrets-and-database/)
4. [Xác thực bằng Cognito](5.4-authentication/)
5. [Deploy backend](5.5-backend/)
6. [S3 private và CloudFront](5.6-storage-delivery/)
7. [Deploy frontend](5.7-frontend/)
8. [Kiểm tra và xử lý lỗi](5.8-validation/)
9. [Dọn dẹp tài nguyên](5.9-cleanup/)

## Ảnh minh họa nên chuẩn bị

Workshop sẽ thuyết phục hơn nếu có ảnh chụp màn hình ở các bước chính. Lưu ảnh
vào thư mục `static/images/workshop/` theo tên gợi ý dưới đây:

| Phần | Nội dung ảnh | Tên file |
|---|---|---|
| 5.1 | Kiến trúc AWS tổng thể | `/images/educloud-aws-architecture.png` |
| 5.4 | Cognito User Pool | `01-cognito-user-pool.png` |
| 5.3 | Supabase Session Pooler | `02-supabase-session-pooler.png` |
| 5.3 | Supabase Table Editor | `02b-supabase-table-editor.png` |
| 5.3 | Parameter Store SecureString | `03-ssm-secure-parameters.png` |
| 5.3 | IAM policy cho EC2 role | `04-eb-ec2-role-ssm-policy.png` |
| 5.5 | Elastic Beanstalk health Green | `05-elastic-beanstalk-green.png` |
| 5.6 | S3 bucket giữ Block Public Access | `06-s3-private-bucket.png` |
| 5.6 | CloudFront origins và behaviors | `07-cloudfront-origins-behaviors.png` |
| 5.7 | Amplify deploy thành công | `08-amplify-deployed.png` |
| 5.8 | Website EduCloud Lite chạy thực tế | `09-live-application.png` |
