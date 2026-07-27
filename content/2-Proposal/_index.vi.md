---
title: "Đề xuất dự án"
weight: 2
chapter: false
pre: "<b>2.</b>"
---

<div class="proposal-hero-title">EDUCLOUD LITE - NỀN TẢNG HỌC TẬP TRÊN AWS</div>

## 1. Tổng quan dự án

EduCloud Lite là nền tảng quản lý học tập trên cloud, cho phép giảng viên tạo
khóa học, tải tài nguyên học tập, cấu hình bài kiểm tra cuối khóa và cấp chứng
chỉ hoàn thành. Sinh viên có thể xem danh sách khóa học đã xuất bản, ghi danh,
học từng bài, theo dõi tiến độ, làm final assessment và nhận chứng chỉ khi đạt
đủ điều kiện.

Dự án tập trung xây dựng một ứng dụng web hoàn chỉnh và triển khai bằng các
dịch vụ AWS theo hướng phù hợp với bài thực tập: kiến trúc rõ ràng, xác thực
được quản lý, lưu trữ private, phân phối qua HTTPS và kiểm soát chi phí.

## 2. Vấn đề cần giải quyết

Các nhóm đào tạo nhỏ hoặc workshop thường cần nhiều hơn một trang tài liệu tĩnh
hoặc danh sách video. Họ cần một hệ thống có thể quản lý người dùng, xuất bản
nội dung theo cấu trúc, theo dõi tiến độ học, đánh giá kết quả cuối khóa và cung
cấp minh chứng hoàn thành.

- **Khó kiểm soát quá trình học:** Trang tĩnh không xác minh được ghi danh, hoàn
  thành bài học, lượt làm bài kiểm tra hoặc điều kiện nhận chứng chỉ.
- **Phân quyền chưa an toàn:** Frontend-only website không đủ để bảo vệ thao tác
  của Instructor và Admin.
- **Tài nguyên bị phân tán:** Video, tài liệu, thumbnail và chứng chỉ thường nằm
  rời rạc, thiếu luồng phân phối nhất quán.
- **Triển khai dễ vượt phạm vi:** Một LMS đầy đủ có thể quá phức tạp hoặc tốn
  chi phí nếu không giới hạn kiến trúc hợp lý.

## 3. Mục tiêu dự án

Mục tiêu là xây dựng một LMS có thể sử dụng thật, bảo mật ở mức phù hợp và trình
diễn được bằng một đường link độc lập.

- **Quản lý khóa học:** Cho phép Instructor tạo khóa học, bài học, thumbnail,
  tài nguyên và final assessment trước khi publish.
- **Luồng học của Student:** Hỗ trợ ghi danh, học bài, lưu tiến độ, nộp bài kiểm
  tra và xem chứng chỉ.
- **Xác thực và phân quyền:** Dùng Amazon Cognito cho danh tính, còn role
  Student, Instructor, Admin được quản lý trong database ứng dụng.
- **Phân phối tài nguyên private:** Lưu file khóa học trong S3 private bucket và
  phân phối qua CloudFront mà không public bucket.
- **Triển khai có thể làm lại:** Viết tài liệu để người khác tự deploy bằng AWS
  account, database và secret của họ.

## 4. Kiến trúc giải pháp

EduCloud Lite sử dụng kiến trúc tách riêng frontend, backend, identity, storage
và database.

![Kiến trúc AWS EduCloud](/images/educloud-aws-architecture.png)

- **Frontend:** React, TypeScript và Vite tạo giao diện người dùng. Amplify
  Hosting build và deploy frontend từ GitHub.
- **Backend:** FastAPI chạy trên Elastic Beanstalk và cung cấp REST API cho khóa
  học, bài học, ghi danh, tiến độ, assessment, certificate, profile, yêu cầu
  Instructor và Admin.
- **Authentication:** Amazon Cognito quản lý mật khẩu, xác nhận tài khoản, đổi
  mật khẩu lần đầu và forgot password. Backend xác thực Cognito token và map
  identity vào user trong Supabase.
- **Database:** Supabase PostgreSQL lưu user, role, course, lesson, enrollment,
  progress, attempt, certificate và instructor request.
- **Storage và delivery:** Amazon S3 lưu file khóa học ở chế độ private.
  CloudFront route `/api/*` đến Elastic Beanstalk và `/courses/*` đến S3 qua
  Origin Access Control.
- **Secrets và vận hành:** Systems Manager Parameter Store lưu secret production.
  IAM role cấp đúng quyền cần thiết cho backend. CloudWatch và Elastic Beanstalk
  health được dùng để kiểm tra vận hành.

## 5. Kế hoạch triển khai

- **Giai đoạn 1 - Chuẩn bị và AWS fundamentals:** Tham gia chương trình, đọc tài
  liệu Cloud Journey, chuẩn bị công cụ, tham gia nhóm thảo luận và chọn đề tài
  EduCloud Lite.
- **Giai đoạn 2 - Nền tảng ứng dụng:** Thiết kế kiến trúc React -> FastAPI ->
  PostgreSQL, xây dựng database model, route và development authentication.
- **Giai đoạn 3 - Chức năng LMS:** Xây dựng course authoring, lesson management,
  enrollment, progress tracking, final assessment, certificate, profile và Admin
  review.
- **Giai đoạn 4 - Tích hợp AWS:** Cấu hình Cognito, Parameter Store, Elastic
  Beanstalk, S3, CloudFront và Amplify Hosting.
- **Giai đoạn 5 - Kiểm thử và báo cáo:** Kiểm thử luồng Student, Instructor,
  Admin; sửa lỗi production; hoàn thiện sơ đồ Draw.io và website báo cáo Hugo.

## 6. Tối ưu chi phí

Kiến trúc được thiết kế để dùng ít credit nhất có thể nhưng vẫn thể hiện được
các dịch vụ AWS quan trọng.

- **Backend single instance:** Elastic Beanstalk dùng môi trường single instance
  để giảm chi phí compute cho lưu lượng demo.
- **S3 private kết hợp CloudFront:** Tài nguyên khóa học được lưu một lần trên
  S3 và phân phối qua CloudFront, thay vì mọi request file đều đi qua backend.
- **Managed identity:** Cognito giúp không phải tự vận hành hệ thống mật khẩu và
  phục hồi tài khoản.
- **PostgreSQL managed bên ngoài:** Supabase được dùng cho bản nộp hiện tại để
  tránh tạo thêm RDS instance.
- **Kiểm soát chi phí:** AWS Budgets, giới hạn logging, không bật WAF khi chưa
  cần và có checklist cleanup để bảo vệ credit.

## 7. Đánh giá rủi ro và hướng xử lý

| Loại rủi ro | Mô tả vấn đề | Hướng xử lý |
| --- | --- | --- |
| Lệch dữ liệu xác thực | Cognito identity và user trong database có thể không khớp. | Map Cognito `sub` với user Supabase và giữ role ứng dụng trong PostgreSQL. |
| Lộ secret | Database URL và JWT secret có thể bị lộ qua code, ảnh chụp hoặc biến frontend. | Lưu secret trong Parameter Store và không đưa vào GitHub, Amplify `VITE_*`, hoặc ảnh báo cáo. |
| Public nhầm tài nguyên | File khóa học có thể bị public nếu cấu hình S3 sai. | Bật Block Public Access, tắt ACL, dùng OAC và giới hạn bucket policy cho CloudFront. |
| Độ trễ khác region | Supabase nằm ngoài `ap-southeast-1`, có thể tăng latency. | Dùng Supabase pooler qua TLS và giữ lưu lượng thấp cho phạm vi bài nộp. |
| Refresh SPA bị 404 | Vào thẳng `/login`, `/profile`, `/instructor` có thể lỗi route. | Cấu hình Amplify rewrite về `/index.html` cho client-side routes. |
| Tăng chi phí AWS | Tài nguyên cloud có thể tiếp tục phát sinh phí sau khi test. | Dùng single instance, AWS Budgets và checklist cleanup theo đúng thứ tự. |

## 8. Kết quả mong đợi

- Website public có thể truy cập qua link Amplify độc lập.
- Backend production chạy trên Elastic Beanstalk và truy cập qua CloudFront.
- Cognito authentication hoạt động cùng role Student, Instructor và Admin.
- Thumbnail, video và tài liệu khóa học được lưu private trong S3 và phân phối
  qua CloudFront.
- Báo cáo workshop hoàn chỉnh gồm kiến trúc, hướng dẫn triển khai,
  troubleshooting, worklog và minh chứng.
