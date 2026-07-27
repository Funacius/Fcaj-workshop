---
title: "Build variables"
weight: 2
chapter: false
pre: "<b>5.7.2.</b>"
---

# Cấu hình build variables

Thêm các Amplify environment variables:

| Key | Value |
| --- | --- |
| `AMPLIFY_MONOREPO_APP_ROOT` | `frontend` |
| `AMPLIFY_DIFF_DEPLOY` | `false` |
| `VITE_API_BASE_URL` | `https://YOUR_CLOUDFRONT_DOMAIN/api` |
| `VITE_COGNITO_REGION` | `ap-southeast-1` |
| `VITE_COGNITO_USER_POOL_ID` | User Pool ID của bạn |
| `VITE_COGNITO_CLIENT_ID` | App client ID của bạn |
| `VITE_ALLOW_LEGACY_AUTH` | `false` |

Các giá trị `VITE_*` được compile vào browser code. Không đưa database URL, JWT
secret, AWS keys hoặc Cognito client secret vào đây.

