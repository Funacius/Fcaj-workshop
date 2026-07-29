---
title: "Kiến trúc hệ thống"
weight: 1
chapter: false
pre: "<b>5.1.1.</b>"
---

# Kiến trúc hệ thống

![Kiến trúc AWS EduCloud](/images/educloud-aws-architecture.png)

EduCloud Lite sử dụng một kiến trúc AWS gọn để phục vụ bài nộp:

- **Frontend:** React, TypeScript và Vite được host trên AWS Amplify, tự động
  deploy từ nhánh `main` của GitHub.
- **Backend:** FastAPI chạy trên Amazon EC2 thông qua Elastic Beanstalk và cung
  cấp API cho khóa học, bài học, ghi danh, tiến độ, bài kiểm tra, review,
  certificate, upload và quản trị.
- **Authentication:** Amazon Cognito xử lý đăng ký, xác minh email, đăng nhập và
  khôi phục mật khẩu. Backend xác thực Cognito token và ánh xạ người dùng với
  role được lưu trong Supabase.
- **Database:** Supabase PostgreSQL lưu users, roles, courses, lessons,
  enrollments, progress, assessments, certificates, reviews và các yêu cầu
  Instructor.
- **Storage và delivery:** Amazon S3 lưu riêng tư thumbnail, video và tài liệu.
  Video được upload trực tiếp bằng presigned URL; CloudFront phân phối course
  assets và route `/api/*` đến Elastic Beanstalk.
- **Monitoring:** Elastic Beanstalk stream application log đến Amazon
  CloudWatch. Admin dashboard hiển thị log gần đây, tình trạng ứng dụng, dung
  lượng S3, thống kê database và thông tin chi phí AWS hiện có.
- **Secrets và permissions:** AWS Systems Manager Parameter Store lưu an toàn
  production secret. Elastic Beanstalk tham chiếu các Parameter Store ARN, còn
  IAM role chỉ cấp quyền cần thiết cho S3, CloudWatch, Cost Explorer và cấu hình
  ứng dụng.
- **Phạm vi:** Tất cả khóa học đều miễn phí. Hệ thống hiện chưa bao gồm pricing,
  checkout hoặc xử lý thanh toán.

Elastic Beanstalk được cấu hình Single instance để kiểm soát chi phí trong phạm
vi bài thực tập. Đây là thiết kế thực tế cho demo, chưa phải thiết kế production
multi-AZ có high availability đầy đủ.

EduCloud chỉ cung cấp khóa học miễn phí. Pricing, checkout và xử lý thanh toán
được chủ động loại khỏi kiến trúc hiện tại.

[Tải file kiến trúc có thể chỉnh sửa](/files/educloud-aws-architecture.drawio)
