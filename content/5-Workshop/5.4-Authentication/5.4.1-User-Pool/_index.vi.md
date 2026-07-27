---
title: "Tạo User Pool"
weight: 1
chapter: false
pre: "<b>5.4.1.</b>"
---

# Tạo User Pool

Tạo Cognito User Pool trong `ap-southeast-1`:

- Sign-in option: email.
- Required attribute: email.
- Password policy: chọn theo yêu cầu workshop.
- Account recovery: self-service recovery, email only.
- MFA: tùy chọn trong phạm vi bài nộp.

Ghi lại:

```text
COGNITO_REGION
COGNITO_USER_POOL_ID
```

![Cognito User Pool](/images/workshop/01-cognito-user-pool.png)

