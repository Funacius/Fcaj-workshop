---
title: "Tuần 9 - Cognito và tăng cường bảo mật"
menuTitle: "Tuần 9"
weight: 9
pre: "<b>1.9.</b>"
---

**Thời gian:** 03/08/2026 - 07/08/2026

## Mục tiêu

- Chuyển authentication production sang Amazon Cognito.
- Hoàn thiện email confirmation, first login và password recovery.
- Tăng cường bảo vệ API và tách rõ identity với role nghiệp vụ.

## Giai đoạn học AWS

| Nội dung học | Nguồn tài liệu | Áp dụng vào EduCloud |
| --- | --- | --- |
| Cognito User Pool, App Client, JWT và luồng xác thực web. | [Amazon Cognito Across Sites](https://000141.awsstudygroup.com/) | Triển khai signup, confirm, login, first login và token exchange. |
| Kết hợp Cognito authentication với frontend và storage access. | [Amplify Authentication and Storage](https://000134.awsstudygroup.com/) | Tách identity do Cognito quản lý khỏi role nghiệp vụ lưu trong Supabase. |

## Công việc thực hiện

| Ngày | Công việc | Kết quả |
| --- | --- | --- |
| 03/08 | Tạo Cognito User Pool/App Client tại Singapore; cấu hình password policy và email recovery. | Cognito quản lý password, confirmation code và reset code. |
| 04/08 | Tích hợp `amazon-cognito-identity-js` cho signup, confirm, resend code và login. | Frontend xử lý đầy đủ vòng đời tài khoản. |
| 05/08 | Xây dựng Cognito ID-token verification/exchange; ánh xạ `sub` vào users.cognito_sub và phát EduCloud JWT. | Role tiếp tục lấy từ Supabase, không tin dữ liệu client. |
| 06/08 | Hoàn thiện NEW_PASSWORD_REQUIRED cho tài khoản được cấp; chuyển người dùng sang Profile sau lần đăng nhập đầu. | Tài khoản tạm có thể tự đặt mật khẩu và hoàn thiện hồ sơ. |
| 07/08 | Tạo forgot/reset password, generic anti-enumeration response, pre-sign-up Lambda tùy chọn; tắt legacy/dev auth production. | Authentication production an toàn và ít làm lộ thông tin tài khoản. |

## Kết quả đạt được

- Cognito chịu trách nhiệm danh tính; Supabase chịu trách nhiệm dữ liệu và role.
- Hỗ trợ xác nhận email, resend code, quên mật khẩu và first-login password.
- Token exchange kiểm tra chữ ký Cognito trước khi phát JWT nội bộ.
- Bổ sung rate limiting, security headers và kiểm tra 401/403.

## Minh chứng từ dự án

- [Cognito service backend](https://github.com/Funacius/EduCloud/blob/main/backend/app/services/cognito_service.py)
- [Cognito service frontend](https://github.com/Funacius/EduCloud/blob/main/frontend/src/services/cognitoService.ts)
- [Pre-sign-up Lambda](https://github.com/Funacius/EduCloud/tree/main/aws/cognito-pre-signup)
- [Cognito exchange tests](https://github.com/Funacius/EduCloud/blob/main/backend/tests/test_cognito_exchange.py)
