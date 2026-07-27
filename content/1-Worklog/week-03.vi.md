---
title: "Tuần 3 - Xây dựng nền tảng backend và database"
menuTitle: "Tuần 3"
weight: 3
pre: "<b>1.3.</b>"
---

**Thời gian:** 22/06/2026 - 26/06/2026

## Mục tiêu

- Kết nối FastAPI với Supabase PostgreSQL bằng SQLAlchemy.
- Xây dựng cấu trúc models, schemas, services và routes.
- Thiết lập cấu hình môi trường, logging và kiểm tra database.

## Giai đoạn học AWS

| Nội dung học | Nguồn tài liệu | Áp dụng vào EduCloud |
| --- | --- | --- |
| IAM User, Policy, Role và nguyên tắc quyền tối thiểu. | [Access Control with AWS IAM](https://000002.awsstudygroup.com/) | Phân biệt quyền quản trị hạ tầng AWS với role Student/Instructor/Admin trong ứng dụng. |
| VPC, subnet, route, Security Group và cách một ứng dụng được truy cập từ Internet. | [Amazon VPC Workshop](https://000003.awsstudygroup.com/) | Thiết kế luồng mạng cho backend và database. |
| EC2/Amazon Linux, triển khai ứng dụng và quản lý tài nguyên compute. | [Introduction to Amazon EC2](https://000004.awsstudygroup.com/) | Hiểu lớp hạ tầng bên dưới Elastic Beanstalk. |
| Cơ sở dữ liệu quan hệ, PostgreSQL, SSL, backup và network isolation. | [Amazon RDS Workshop](https://000005.awsstudygroup.com/) | Thiết kế schema SQLAlchemy và kết nối Supabase PostgreSQL an toàn. |

## Công việc thực hiện

| Ngày | Công việc | Kết quả |
| --- | --- | --- |
| 22/06 | Thiết kế kiến trúc React → FastAPI → PostgreSQL và xác định ranh giới frontend/backend/database. | Có sơ đồ request flow và stack kỹ thuật cho EduCloud. |
| 23/06 | Xây dựng `config.py`, `.env.example`; cấu hình SQLAlchemy với chuỗi kết nối Supabase SSL. | Backend kết nối được PostgreSQL qua session pooler. |
| 24/06 | Tạo các model nền tảng cho user, course, lesson, enrollment và progress. | Database phản ánh được các quan hệ nghiệp vụ chính. |
| 25/06 | Xây dựng Pydantic schemas, service layer, router và response helper. | API có kiến trúc nhất quán, dễ kiểm thử và mở rộng. |
| 26/06 | Thêm startup table creation, compatibility migrations, script kiểm tra database và seed tài khoản phát triển. | Có thể khởi tạo môi trường mới bằng các lệnh tái sử dụng. |

## Kết quả đạt được

- FastAPI kết nối ổn định với Supabase PostgreSQL bằng `psycopg2`.
- Hoàn thiện cấu trúc backend theo hướng router → service → model.
- Có script kiểm tra kết nối mà không làm lộ mật khẩu.
- Chuẩn bị dữ liệu mẫu cho Student, Instructor và Admin.

## Minh chứng từ dự án

- [Database configuration](https://github.com/Funacius/EduCloud/blob/main/backend/app/database.py)
- [Compatibility migrations](https://github.com/Funacius/EduCloud/blob/main/backend/app/database_migrations.py)
- [Database check script](https://github.com/Funacius/EduCloud/blob/main/backend/scripts/check_database.py)
