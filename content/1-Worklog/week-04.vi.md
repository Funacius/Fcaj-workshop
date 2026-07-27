---
title: "Tuần 4 - Xác thực cơ bản và phân quyền"
menuTitle: "Tuần 4"
weight: 4
pre: "<b>1.4.</b>"
---

**Thời gian:** 29/06/2026 - 03/07/2026

## Mục tiêu

- Xây dựng luồng đăng ký, đăng nhập và JWT cho môi trường phát triển.
- Bảo vệ API và giao diện theo vai trò người dùng.
- Chuẩn bị lớp abstraction để chuyển sang Cognito ở giai đoạn production.

## Giai đoạn học AWS

| Nội dung học | Nguồn tài liệu | Áp dụng vào EduCloud |
| --- | --- | --- |
| IAM Policy, Role, temporary access và nguyên tắc least privilege. | [Access Control with AWS IAM](https://000002.awsstudygroup.com/) | Thiết kế authorization helper và giới hạn API theo vai trò. |
| User Pool, JWT và sự khác nhau giữa xác thực người dùng với cấp quyền truy cập tài nguyên AWS. | [Amazon Cognito Across Sites](https://000141.awsstudygroup.com/) | Chuẩn bị kiến trúc chuyển từ local JWT sang Cognito ở production. |

## Công việc thực hiện

| Ngày | Công việc | Kết quả |
| --- | --- | --- |
| 29/06 | Xây dựng user schema, password hashing và API đăng ký/đăng nhập development. | Có thể tạo và xác thực tài khoản local an toàn. |
| 30/06 | Tạo JWT chứa user id, email, role và thời hạn; triển khai xác thực Bearer token. | Backend nhận diện được người dùng ở mỗi request. |
| 01/07 | Xây dựng auth middleware và authorization helper cho Student, Instructor, Admin. | Các API mutation được giới hạn đúng quyền. |
| 02/07 | Tạo `AuthContext`, lưu session, tự đính kèm token và `RequireRole` ở frontend. | Điều hướng và trang riêng được bảo vệ theo role. |
| 03/07 | Thử truy cập chéo vai trò; bổ sung security headers, lỗi 401/403 và giới hạn auth cơ bản. | Giảm khả năng leo thang quyền và lộ route nhạy cảm. |

## Kết quả đạt được

- Hoàn thành authentication development và JWT 12 giờ.
- Các route Instructor/Admin được bảo vệ ở cả frontend lẫn backend.
- Public registration không thể tự gán quyền Admin.
- Cấu trúc cho phép chuyển sang Cognito mà không thay đổi toàn bộ ứng dụng.

## Minh chứng từ dự án

- [Authentication routes](https://github.com/Funacius/EduCloud/blob/main/backend/app/routes/auth_routes.py)
- [Authorization middleware](https://github.com/Funacius/EduCloud/blob/main/backend/app/middleware/auth_middleware.py)
- [Frontend authentication context](https://github.com/Funacius/EduCloud/blob/main/frontend/src/auth/AuthContext.tsx)
