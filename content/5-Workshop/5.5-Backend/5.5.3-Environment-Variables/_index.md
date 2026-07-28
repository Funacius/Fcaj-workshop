---
title: "Environment Variables"
weight: 3
chapter: false
pre: "<b>5.5.3.</b>"
---

# Configure Environment Variables

Configure the two secret-backed values from Parameter Store and the remaining
plain-text settings:

| Name | Source / example |
| --- | --- |
| `DATABASE_URL` | Parameter Store ARN |
| `JWT_SECRET_KEY` | Parameter Store ARN |
| `APP_ENV` | `production` |
| `API_PREFIX` | `/api` |
| `JWT_ALGORITHM` | `HS256` |
| `AWS_REGION` | `ap-southeast-1` |
| `COGNITO_REGION` | `ap-southeast-1` |
| `COGNITO_USER_POOL_ID` | Your User Pool ID |
| `COGNITO_CLIENT_ID` | Your app client ID |
| `ALLOW_LEGACY_AUTH` | `false` |
| `ENABLE_DEV_AUTH` | `false` |
| `UPLOAD_STORAGE` | `s3` after Module 5.6 |
| `AWS_S3_BUCKET_NAME` | Your private upload bucket |
| `AWS_S3_PUBLIC_BASE_URL` | Your CloudFront distribution URL |
| `AWS_MONITORING_ENABLED` | `true` to enable AWS billing/log reads |
| `AWS_CLOUDWATCH_LOG_GROUP` | Exact EB `web.stdout.log` group name |
| `CORS_ORIGINS` | Your Amplify domain |

When these settings change, wait for Elastic Beanstalk to finish updating before
testing the API again.

![Elastic Beanstalk environment properties using Parameter Store](/images/workshop/05b-eb-env-parameter-store.png)
