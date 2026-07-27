---
title: "Tuần 6 - Ghi danh, học tập và theo dõi tiến độ"
menuTitle: "Tuần 6"
weight: 6
pre: "<b>1.6.</b>"
---

**Thời gian:** 13/07/2026 - 17/07/2026

## Mục tiêu

- Xây dựng catalog khóa học công khai và luồng ghi danh.
- Tạo Learning Page cho Student học theo curriculum.
- Lưu tiến độ từng bài học và tính phần trăm hoàn thành.

## Giai đoạn học AWS

| Nội dung học | Nguồn tài liệu | Áp dụng vào EduCloud |
| --- | --- | --- |
| Luồng frontend đăng nhập, truy cập API và sử dụng storage theo access level. | [Amplify Authentication and Storage](https://000134.awsstudygroup.com/) | Thiết kế điều kiện chỉ người đã ghi danh mới truy cập nội dung học. |
| Cache nội dung tĩnh tại edge và bảo vệ origin S3. | [CloudFront with S3 Origin](https://000094.awsstudygroup.com/) | Chuẩn bị hướng phân phối video, thumbnail và tài liệu cho Learning Page. |

## Công việc thực hiện

| Ngày | Công việc | Kết quả |
| --- | --- | --- |
| 13/07 | Xây dựng course catalog, tìm kiếm, course card và trang chi tiết khóa học. | Người dùng xem được khóa học Published từ Supabase. |
| 14/07 | Triển khai enrollment service/API với unique constraint và kiểm tra role Student. | Ghi danh idempotent, không tạo bản ghi trùng. |
| 15/07 | Xây dựng Learning Page gồm playlist bài học, video, notes và tài liệu. | Student đã ghi danh mới xem được nội dung đầy đủ. |
| 16/07 | Tạo progress API để complete/undo lesson; tính số bài và phần trăm hoàn thành. | Tiến độ được lưu theo từng Student và lesson. |
| 17/07 | Xây dựng My Learning dashboard và resume từ bài chưa hoàn thành đầu tiên; kiểm tra thống kê Instructor. | Student tiếp tục học đúng vị trí và Instructor thấy số ghi danh. |

## Kết quả đạt được

- Public API chỉ trả outline, không làm lộ video và tài liệu riêng tư.
- Student ghi danh và theo dõi tiến độ trên Supabase.
- Learning Page hỗ trợ chuyển bài và đánh dấu hoàn thành.
- Dashboard phản ánh dữ liệu thật thay vì số liệu cố định.

## Minh chứng từ dự án

- [Enrollment service](https://github.com/Funacius/EduCloud/blob/main/backend/app/services/enrollment_service.py)
- [Progress service](https://github.com/Funacius/EduCloud/blob/main/backend/app/services/progress_service.py)
- [Learning Page](https://github.com/Funacius/EduCloud/blob/main/frontend/src/pages/LearningPage.tsx)
