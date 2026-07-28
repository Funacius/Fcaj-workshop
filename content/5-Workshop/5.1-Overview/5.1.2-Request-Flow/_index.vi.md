---
title: "Luồng request"
weight: 2
chapter: false
pre: "<b>5.1.2.</b>"
---

# Luồng request

## Các luồng chính

1. Push lên nhánh `main` kích hoạt Amplify build.
2. Người dùng tải React/Vite SPA từ Amplify Hosting.
3. Trình duyệt xác thực trực tiếp với Cognito User Pool.
4. Trình duyệt gửi API request và media request tới CloudFront.
5. CloudFront chuyển API request tới Elastic Beanstalk.
6. CloudFront chuyển request `/courses/*` tới S3 private bằng OAC.
7. FastAPI đọc/ghi dữ liệu trong Supabase PostgreSQL qua TLS.
8. FastAPI cấp quyền object private và trả multipart presigned URL có thời hạn
   ngắn; trình duyệt upload từng phần video trực tiếp lên S3.
9. Object trong S3 được phân phối qua CloudFront, còn Elastic Beanstalk stream
   application log sang CloudWatch.
10. EC2 instance profile cấp quyền cho backend đọc encrypted parameters, truy
    cập upload bucket, Cost Explorer và CloudWatch log group đã cấu hình.

## Ý đồ thiết kế

Luồng request tách riêng frontend hosting, authentication, backend execution,
private media delivery và database access. Nhờ đó, khi có lỗi, có thể khoanh vùng
theo từng lớp: Amplify, Cognito, CloudFront, Elastic Beanstalk, S3, Parameter
Store hoặc PostgreSQL.
