---
title: "Tuần 12 - Kiểm thử, tối ưu và hoàn thiện báo cáo"
menuTitle: "Tuần 12"
weight: 12
pre: "<b>1.12.</b>"
---

**Thời gian:** 24/08/2026 - 15/09/2026

## Mục tiêu

- Kiểm thử end-to-end toàn bộ ba vai trò trên production.
- Sửa các lỗi UI, authentication, CORS, upload và deployment.
- Hoàn thiện kiến trúc Draw.io, workshop Hugo và hồ sơ nộp bài.

## Giai đoạn học và rà soát vận hành

| Nội dung học | Nguồn tài liệu | Áp dụng vào EduCloud |
| --- | --- | --- |
| Metrics, logs, alarms và dashboard theo dõi ứng dụng. | [AWS CloudWatch Workshop](https://000008.awsstudygroup.com/) | Kiểm tra health, log Elastic Beanstalk và lập danh sách chỉ số vận hành. |
| Budget alert, theo dõi chi phí và dọn dẹp tài nguyên sau workshop. | [AWS Budgets](https://000007.awsstudygroup.com/) | Kiểm soát credit, tắt tài nguyên không dùng và hoàn thiện mục cleanup. |
| Rà soát toàn bộ lộ trình AWS đã áp dụng cho dự án. | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/) | Đối chiếu kiến trúc EduCloud với các workshop đã học. |

## Công việc thực hiện

| Mốc | Công việc | Kết quả |
| --- | --- | --- |
| 24/08 | Chạy backend pytest bằng SQLite isolated; kiểm tra Cognito exchange, course/lesson, enrollment/progress, instructor request và monitoring. | Xác nhận các luồng backend không làm thay đổi Supabase production. |
| 28/08 | Chạy `npm run build`; kiểm thử responsive, role navigation, course authoring, thumbnail crop và draft deletion. | Frontend TypeScript build thành công và UI ổn định hơn. |
| 03/09 | Kiểm thử Student end-to-end: đăng nhập, profile, ghi danh, học, assessment, certificate và resume learning. | Xác nhận điều kiện hoàn thành khóa học hoạt động đúng. |
| 07/09 | Kiểm thử Instructor/Admin và production; sửa CORS, SPA rewrite, CloudFront behavior, environment variables và auth state. | Website hoạt động qua link độc lập, điều hướng không lỗi khi refresh. |
| 15/09 | Hoàn thiện Draw.io, ảnh minh chứng, workshop song ngữ, worklog, self-assessment, cleanup và kiểm tra link. | Sẵn sàng nộp repository, live website và Hugo report. |

## Kết quả đạt được

- Có bộ test backend cho các nghiệp vụ và ràng buộc bảo mật quan trọng.
- Xác thực đủ luồng Student, Instructor và Admin.
- Hoàn thiện sơ đồ kiến trúc AWS cùng request flow.
- Hoàn thành website Hugo theo theme First Cloud Journey và tài liệu triển khai.
- Ghi lại hạn chế còn lại: Alembic production, server-side certificate PDF,
  shared monitoring và hardening token/cookie.

## Minh chứng từ dự án

- [Backend tests](https://github.com/Funacius/EduCloud/tree/main/backend/tests)
- [API test plan](https://github.com/Funacius/EduCloud/blob/main/api/test-plan/test-cases.md)
- [Architecture source](https://github.com/Funacius/EduCloud/blob/main/docs/architecture/educloud-aws-architecture.drawio)
- [Project documentation](https://github.com/Funacius/EduCloud/blob/main/README.md)
