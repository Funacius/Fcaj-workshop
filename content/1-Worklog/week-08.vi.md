---
title: "Tuần 8 - Hồ sơ, xét duyệt giảng viên và Admin"
menuTitle: "Tuần 8"
weight: 8
pre: "<b>1.8.</b>"
---

**Thời gian:** 27/07/2026 - 31/07/2026

## Mục tiêu

- Hoàn thiện hồ sơ Student dùng trên chứng chỉ.
- Xây dựng quy trình đăng ký trở thành Instructor có Admin xét duyệt.
- Thay số liệu demo bằng dashboard lấy từ database.

## Giai đoạn học AWS

| Nội dung học | Nguồn tài liệu | Áp dụng vào EduCloud |
| --- | --- | --- |
| IAM group, role, policy và mô hình cấp quyền có kiểm soát. | [Access Control with AWS IAM](https://000002.awsstudygroup.com/) | Đối chiếu với quy trình Admin duyệt Instructor và audit người xét duyệt. |
| CloudWatch metrics, logs và dashboard vận hành. | [AWS CloudWatch Workshop](https://000008.awsstudygroup.com/) | Xác định các chỉ số traffic, latency, database và storage trên Admin Health Page. |

## Công việc thực hiện

| Ngày | Công việc | Kết quả |
| --- | --- | --- |
| 27/07 | Tạo student_profiles và API đọc/cập nhật tên chứng chỉ, ngày sinh, tổ chức, quốc gia, bio. | Student chủ động hoàn thiện danh tính trước khi học. |
| 28/07 | Thiết kế instructor_requests với pending/approved/rejected, review note và reviewer audit. | Có quy trình nâng quyền có kiểm soát. |
| 29/07 | Xây dựng UI Become an Instructor, xem lý do từ chối và gửi lại đơn. | Student theo dõi và resubmit yêu cầu. |
| 30/07 | Phát triển Admin Dashboard: users, roles, courses, lessons, enrollments, application queue và course oversight. | Admin quản trị từ dữ liệu Supabase thật. |
| 31/07 | Tạo Admin Health Page cho traffic, latency, database, storage và service status; bổ sung refresh. | Có màn hình theo dõi tình trạng hệ thống. |

## Kết quả đạt được

- Tách profile chứng chỉ khỏi dữ liệu đăng nhập.
- Instructor chỉ được kích hoạt sau khi Admin approve.
- Rejected request bắt buộc có ghi chú và có thể gửi lại.
- Admin xem thống kê, kiểm duyệt khóa học và theo dõi sức khỏe hệ thống.

## Minh chứng từ dự án

- [Profile routes](https://github.com/Funacius/EduCloud/blob/main/backend/app/routes/profile_routes.py)
- [Instructor request service](https://github.com/Funacius/EduCloud/blob/main/backend/app/services/instructor_request_service.py)
- [Admin dashboard](https://github.com/Funacius/EduCloud/blob/main/frontend/src/pages/AdminDashboardPage.tsx)
- [Admin health page](https://github.com/Funacius/EduCloud/blob/main/frontend/src/pages/AdminHealthPage.tsx)
