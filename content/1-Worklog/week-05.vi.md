---
title: "Tuần 5 - Quản lý khóa học, bài học và tài nguyên"
menuTitle: "Tuần 5"
weight: 5
pre: "<b>1.5.</b>"
---

**Thời gian:** 06/07/2026 - 10/07/2026

## Mục tiêu

- Xây dựng quy trình Instructor tạo và quản lý khóa học.
- Tổ chức curriculum theo bài học có thứ tự.
- Hỗ trợ thumbnail, video và tài liệu học tập.

## Giai đoạn học AWS

| Nội dung học | Nguồn tài liệu | Áp dụng vào EduCloud |
| --- | --- | --- |
| Mô hình lưu object, upload file và phân tách dữ liệu có cấu trúc với tài nguyên nhị phân. | [S3 Static Website Hosting](https://000057.awsstudygroup.com/) | Thiết kế abstraction để thumbnail, video và tài liệu có thể chuyển từ local sang S3. |
| Access level cho file và kết hợp authentication với storage. | [Amplify Authentication and Storage](https://000134.awsstudygroup.com/) | Xác định quyền upload của Instructor và quyền đọc của Student. |

## Công việc thực hiện

| Ngày | Công việc | Kết quả |
| --- | --- | --- |
| 06/07 | Xây dựng model, schema, service và CRUD API cho course; bổ sung ownership check. | Instructor chỉ sửa được khóa học do mình sở hữu. |
| 07/07 | Phát triển trang danh sách, tạo và chỉnh sửa khóa học với title, description, level, category, tags, price và status. | Có Instructor workspace hoàn chỉnh. |
| 08/07 | Xây dựng lesson CRUD, order index, notes, video URL và material URL. | Instructor quản lý được curriculum theo thứ tự. |
| 09/07 | Tạo Course Curriculum Editor; xử lý thêm, sửa, xóa bài học và validation dữ liệu. | Luồng biên soạn nội dung hoạt động từ UI tới database. |
| 10/07 | Thêm upload abstraction ở chế độ local, thumbnail preview và công cụ crop ảnh 4:3. | Có thể tải ảnh, video, tài liệu và chuẩn hóa thumbnail 1200×900. |

## Kết quả đạt được

- Hoàn thiện CRUD khóa học và bài học có kiểm tra ownership.
- Tách trạng thái Draft, Published và Hidden.
- Hỗ trợ curriculum có thứ tự cùng nội dung riêng tư.
- Xây dựng thumbnail crop editor với kéo, zoom và thay ảnh.

## Minh chứng từ dự án

- [Course routes](https://github.com/Funacius/EduCloud/blob/main/backend/app/routes/course_routes.py)
- [Curriculum editor](https://github.com/Funacius/EduCloud/blob/main/frontend/src/components/CourseCurriculumEditor.tsx)
- [Thumbnail crop editor](https://github.com/Funacius/EduCloud/blob/main/frontend/src/components/ThumbnailCropEditor.tsx)
