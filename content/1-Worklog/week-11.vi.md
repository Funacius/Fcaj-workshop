---
title: "Tuần 11 - S3, CloudFront và Amplify Hosting"
menuTitle: "Tuần 11"
weight: 11
pre: "<b>1.11.</b>"
---

**Thời gian:** 17/08/2026 - 21/08/2026

## Mục tiêu

- Chuẩn bị lưu trữ tài nguyên khóa học trên S3 private.
- Phân phối nội dung qua CloudFront và backend qua HTTPS.
- Triển khai React frontend tự động từ GitHub.

## Giai đoạn học AWS

| Nội dung học | Nguồn tài liệu | Áp dụng vào EduCloud |
| --- | --- | --- |
| HTTPS, SSE-S3, Block Public Access, bucket policy và kiểm tra quyền truy cập. | [S3 Security Best Practices](https://000069.awsstudygroup.com/) | Giữ bucket tài nguyên khóa học ở chế độ private. |
| S3 origin, CloudFront cache và phân phối nội dung qua CDN. | [CloudFront with S3 Origin](https://000094.awsstudygroup.com/) | Tạo OAC và behavior riêng cho `courses/*`. |
| Hosting frontend, authentication/storage integration và access level. | [Amplify Authentication and Storage](https://000134.awsstudygroup.com/) | Cấu hình Amplify Hosting, biến `VITE_*` và SPA rewrite. |
| SSL, CloudFront và kiến trúc website tĩnh an toàn. | [S3 Static SSL Website](https://000137.awsstudygroup.com/) | Kiểm tra HTTPS và định hướng custom domain sau này. |

## Công việc thực hiện

| Ngày | Công việc | Kết quả |
| --- | --- | --- |
| 17/08 | Tạo bucket `educloud-uploads` tại Singapore; bật Block Public Access, ACL disabled và SSE-S3. | Tài nguyên không được công khai trực tiếp từ S3. |
| 18/08 | Hoàn thiện `s3_service.py`, upload validation và biến `UPLOAD_STORAGE`; cấp policy theo bucket cho backend role. | Backend có abstraction để chuyển local/S3 mà không đổi API. |
| 19/08 | Tạo CloudFront S3 origin với OAC và bucket policy; cấu hình behavior `courses/*`, cache và SimpleCORS. | CloudFront được phép đọc object private. |
| 20/08 | Cấu hình Elastic Beanstalk origin/API behavior với cache disabled, AllViewer và đầy đủ HTTP methods. | Frontend gọi API qua HTTPS cùng một distribution. |
| 21/08 | Push source lên GitHub; cấu hình Amplify monorepo root, `amplify.yml`, VITE variables và SPA rewrite. | React production build tự động từ nhánh `main`. |

## Kết quả đạt được

- Bucket giữ private, chỉ CloudFront OAC và backend role có quyền cần thiết.
- Upload service kiểm tra loại file và kích thước thumbnail/material/video.
- CloudFront tách cache behavior cho static assets và API.
- Amplify triển khai frontend từ GitHub và hỗ trợ React client routes.

## Minh chứng từ dự án

- [S3 service](https://github.com/Funacius/EduCloud/blob/main/backend/app/services/s3_service.py)
- [Upload routes](https://github.com/Funacius/EduCloud/blob/main/backend/app/routes/upload_routes.py)
- [Amplify build configuration](https://github.com/Funacius/EduCloud/blob/main/amplify.yml)
- [AWS architecture](https://github.com/Funacius/EduCloud/tree/main/docs/architecture)
