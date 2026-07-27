---
title: "Auto Confirm Lambda"
weight: 3
chapter: false
pre: "<b>5.4.3.</b>"
---

# Auto Confirm Lambda tùy chọn

Repository có file:

```text
aws/cognito-pre-signup/index.mjs
```

Nếu muốn self-service signup được confirm tự động:

1. Tạo Node.js Lambda function cùng Region.
2. Dán code và deploy function.
3. Gắn function vào User Pool ở **Pre sign-up Lambda trigger**.

Trigger này confirm và verify self-service signup. Nó không thay thế email dùng
cho Forgot Password flow.

Nếu muốn người dùng tự xác nhận đăng ký, không gắn trigger này. Trong cả hai
trường hợp vẫn nên bật email recovery.

Role ứng dụng vẫn lưu trong PostgreSQL:

- Người dùng public mới là Student.
- Instructor applicant vẫn là Student cho đến khi Admin duyệt.
- Admin account phải được cấp riêng.

