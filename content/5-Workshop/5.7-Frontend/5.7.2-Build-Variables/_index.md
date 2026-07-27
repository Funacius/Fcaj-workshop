---
title: "Build Variables"
weight: 2
chapter: false
pre: "<b>5.7.2.</b>"
---

# Configure Build Variables

Add these Amplify environment variables:

| Key | Value |
| --- | --- |
| `AMPLIFY_MONOREPO_APP_ROOT` | `frontend` |
| `AMPLIFY_DIFF_DEPLOY` | `false` |
| `VITE_API_BASE_URL` | `https://YOUR_CLOUDFRONT_DOMAIN/api` |
| `VITE_COGNITO_REGION` | `ap-southeast-1` |
| `VITE_COGNITO_USER_POOL_ID` | Your User Pool ID |
| `VITE_COGNITO_CLIENT_ID` | Your app client ID |
| `VITE_ALLOW_LEGACY_AUTH` | `false` |

These `VITE_*` values are compiled into browser code. Never put the database URL,
JWT secret, AWS keys, or a Cognito client secret here.

