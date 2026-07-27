---
title: "Tuần 10 - Secrets và triển khai backend"
menuTitle: "Tuần 10"
weight: 10
pre: "<b>1.10.</b>"
---

**Thời gian:** 10/08/2026 - 14/08/2026

## Mục tiêu

- Đưa secret production ra khỏi source code.
- Triển khai FastAPI lên AWS Elastic Beanstalk.
- Kiểm tra health, logging và kết nối Supabase từ môi trường AWS.

## Giai đoạn học AWS

| Nội dung học | Nguồn tài liệu | Áp dụng vào EduCloud |
| --- | --- | --- |
| Quản lý thông tin kết nối database và secret ngoài source code. | [Secrets Manager with RDS](https://000096.awsstudygroup.com/) | Áp dụng cùng nguyên tắc bằng SSM Parameter Store cho `DATABASE_URL` và `JWT_SECRET_KEY`. |
| IAM instance role, môi trường development/production và cập nhật ứng dụng trên Elastic Beanstalk. | [Deploy Application Using Elastic Beanstalk](https://000112.awsstudygroup.com/) | Triển khai FastAPI trên Python 3.12/Amazon Linux 2023. |

## Công việc thực hiện

| Ngày | Công việc | Kết quả |
| --- | --- | --- |
| 10/08 | Tạo SSM Parameter Store cho `DATABASE_URL` và `JWT_SECRET_KEY` ở `ap-southeast-1`. | Secret được mã hóa và quản lý ngoài repository. |
| 11/08 | Tạo service role, EC2 instance profile và inline policy chỉ đọc đúng hai parameter. | Elastic Beanstalk có quyền tối thiểu cần thiết. |
| 12/08 | Chuẩn bị `Procfile`, requirements, port 8000, application entry point và gói deploy backend. | Source bundle phù hợp Python 3.12 trên Amazon Linux 2023. |
| 13/08 | Tạo Single Instance Elastic Beanstalk environment; cấu hình Cognito, CORS, production flags và SSM references. | Backend được deploy và environment đạt trạng thái Green. |
| 14/08 | Kiểm tra `/`, `/docs`, API health, database SSL, log `web.stdout`; sửa dependency và startup errors. | Backend production truy cập được và kết nối Supabase ổn định. |

## Kết quả đạt được

- Không đưa database password hoặc JWT secret vào GitHub/Amplify.
- Elastic Beanstalk đọc secret bằng instance role.
- FastAPI chạy qua Nginx/uvicorn và báo Green health.
- Có quy trình kiểm tra log và redeploy khi thay đổi backend.

## Minh chứng từ dự án

- [Backend Procfile](https://github.com/Funacius/EduCloud/blob/main/backend/Procfile)
- [Production configuration](https://github.com/Funacius/EduCloud/blob/main/backend/app/config.py)
- [Backend deployment instructions](https://github.com/Funacius/EduCloud/blob/main/backend/README.md)
