---
title: "Cấu hình App Client"
weight: 2
chapter: false
pre: "<b>5.4.2.</b>"
---

# Cấu hình App Client

Tạo public app client cho React frontend:

- App type: public client.
- Client secret: disabled.
- Authentication flows: username/password hoặc cấu hình mặc định phù hợp.
- Callback/sign-out URLs: thêm Amplify domain nếu dùng hosted UI.

Ghi lại:

```text
COGNITO_CLIENT_ID
```

Frontend dùng `VITE_COGNITO_REGION`, `VITE_COGNITO_USER_POOL_ID` và
`VITE_COGNITO_CLIENT_ID`. Đây là các public identifier; không đưa Cognito client
secret vào ứng dụng chạy trên trình duyệt.

