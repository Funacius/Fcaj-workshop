---
title: "Environment variables"
weight: 3
chapter: false
pre: "<b>5.5.3.</b>"
---

# Cấu hình environment variables

Cấu hình hai giá trị secret-backed từ Parameter Store và các setting plain-text:

| Name | Source / example |
| --- | --- |
| `DATABASE_URL` | Parameter Store ARN |
| `JWT_SECRET_KEY` | Parameter Store ARN |
| `APP_ENV` | `production` |
| `API_PREFIX` | `/api` |
| `JWT_ALGORITHM` | `HS256` |
| `AWS_REGION` | `ap-southeast-1` |
| `COGNITO_REGION` | `ap-southeast-1` |
| `COGNITO_USER_POOL_ID` | User Pool ID của bạn |
| `COGNITO_CLIENT_ID` | App client ID của bạn |
| `ALLOW_LEGACY_AUTH` | `false` |
| `ENABLE_DEV_AUTH` | `false` |
| `UPLOAD_STORAGE` | `s3` sau Module 5.6 |
| `AWS_S3_BUCKET_NAME` | Private upload bucket của bạn |
| `AWS_S3_PUBLIC_BASE_URL` | URL CloudFront distribution |
| `AWS_MONITORING_ENABLED` | `true` để bật đọc billing/log AWS |
| `AWS_CLOUDWATCH_LOG_GROUP` | Tên chính xác của EB `web.stdout.log` group |
| `CORS_ORIGINS` | Amplify domain của bạn |

Khi thay đổi các setting này, chờ Elastic Beanstalk cập nhật xong rồi mới test
API lại.

![Elastic Beanstalk environment properties dùng Parameter Store](/images/workshop/05b-eb-env-parameter-store.png)
