---
title: "Tuần 8 - Kiểm tra cuối, hoàn thiện báo cáo và nộp bài"
menuTitle: "Tuần 8"
weight: 8
pre: "<b>1.8.</b>"
---

**Thời gian:** 27/07/2026 - 31/07/2026

**Hạn nộp workshop:** 31/07/2026

## Mục tiêu

- Đóng phạm vi tính năng EduCloud Lite và không bổ sung thay đổi rủi ro trước khi nộp.
- Kiểm tra lại luồng Student, Instructor, các dịch vụ AWS và toàn bộ public link.
- Hoàn thiện Hugo workshop song ngữ, ảnh minh chứng, sơ đồ kiến trúc và hồ sơ nộp bài.
- Nộp workshop chậm nhất vào ngày 31/07/2026.

## Kiểm tra cuối project và AWS

| Phạm vi kiểm tra | Hoạt động xác minh | Minh chứng nộp bài |
| --- | --- | --- |
| Danh tính và bảo mật | Kiểm tra Cognito login, đổi mật khẩu lần đầu, recovery, JWT exchange, IAM role và secret trong Parameter Store. | Screenshot xác thực, ghi chú cấu hình và link mã nguồn. |
| Hosting ứng dụng | Kiểm tra Elastic Beanstalk health, Amplify deployment, CloudFront routing, CORS và SPA rewrite. | Link EduCloud public và trạng thái triển khai thành công. |
| Lưu trữ và dữ liệu | Xác minh phân phối S3 private, kết nối Supabase, đường dẫn upload và role lấy từ database. | Sơ đồ kiến trúc và ảnh trong workshop. |
| Tài liệu | Rà soát proposal, worklog, blogs, events, workshop, self-assessment và feedback ở cả hai ngôn ngữ. | Hugo report public và GitHub repository. |

## Kế hoạch công việc và tiến độ hiện tại

| Ngày | Công việc | Kết quả cần đạt |
| --- | --- | --- |
| 27/07 | Sửa thời gian thực tập và phạm vi worklog; hoàn thiện nội dung báo cáo và minh chứng còn thiếu. | Báo cáo phản ánh đúng kỳ thực tập 01/06-15/08. |
| 28/07 | Chạy checklist production cho login, xem khóa học, ghi danh, học bài, assessment, certificate và Instructor authoring. | Các luồng demo quan trọng hoạt động qua website public. |
| 29/07 | Rà soát sơ đồ AWS, screenshot, code snippet, hướng dẫn repository và tài khoản demo. | Mentor có thể hiểu và làm theo nội dung nộp bài. |
| 30/07 | Đọc lại bản Anh/Việt; kiểm tra navigation, dấu tích đã đọc, hình ảnh, liên kết và giao diện responsive. | Hugo report thống nhất, không còn placeholder hoặc ảnh lỗi. |
| 31/07 | Chuẩn bị hồ sơ cuối và gửi website, repository mã nguồn, workshop cùng các file PDF được chọn. | Nộp workshop đúng hạn. |

## Kết quả hiện tại

- EduCloud Lite đã có đường dẫn độc lập qua Amplify.
- Backend FastAPI chạy trên Elastic Beanstalk và đọc secret production từ Parameter Store.
- Cognito, Supabase, S3, CloudFront và Amplify đã được kết nối với ứng dụng triển khai.
- Sơ đồ kiến trúc, Hugo report song ngữ, link demo và tài khoản cho mentor đã được ghi lại.
- Công việc còn lại chỉ gồm kiểm tra cuối, rà soát minh chứng, sửa nội dung và nộp bài.

## Liên kết nộp bài

- [Website EduCloud Lite](https://main.djk00b5qbck73.amplifyapp.com/courses)
- [Repository mã nguồn EduCloud](https://github.com/Funacius/EduCloud)
- [Báo cáo thực tập public](https://funacius.github.io/Fcaj-workshop/vi/)
- [Repository Hugo report](https://github.com/Funacius/Fcaj-workshop)

> Sau ngày 31/07 không mở rộng thêm tính năng project. Thời gian thực tập còn lại chỉ dành cho việc học AWS và First Cloud AI Journey.
