---
title: "Tuần 7 - Final assessment và chứng chỉ"
menuTitle: "Tuần 7"
weight: 7
pre: "<b>1.7.</b>"
---

**Thời gian:** 20/07/2026 - 24/07/2026

## Mục tiêu

- Cho phép Instructor cấu hình bài đánh giá cuối khóa.
- Chấm điểm và kiểm soát thời gian, số lần thi ở backend.
- Chỉ cấp chứng chỉ khi hoàn thành bài học và thi đạt.

## Giai đoạn học AWS

| Nội dung học | Nguồn tài liệu | Áp dụng vào EduCloud |
| --- | --- | --- |
| Metrics, logs, alarms và dashboard cho ứng dụng. | [AWS CloudWatch Workshop](https://000008.awsstudygroup.com/) | Xác định dữ liệu cần theo dõi cho lượt làm bài, lỗi submit và latency. |
| Thiết kế ứng dụng có trạng thái nhất quán, xử lý retry và thao tác idempotent. | [Application Modernization on AWS](https://cloudjourney.awsstudygroup.com/) | Bảo đảm submit bài và cấp chứng chỉ không tạo bản ghi trùng. |

## Công việc thực hiện

| Ngày | Công việc | Kết quả |
| --- | --- | --- |
| 20/07 | Thiết kế bảng course_assessments, assessment_questions và assessment_attempts. | Database lưu được cấu hình, câu hỏi và lịch sử làm bài. |
| 21/07 | Xây dựng Assessment Editor với thời gian, điểm đạt, số lần thi, publish và nhiều hơn bốn đáp án. | Instructor tạo được final assessment linh hoạt. |
| 22/07 | Thêm chế độ một đáp án, chọn tất cả đáp án đúng hoặc chọn một trong các đáp án đúng; hỗ trợ kéo thả option. | Câu hỏi đáp ứng nhiều cách chấm khác nhau. |
| 23/07 | Xây dựng Student Assessment Page với timer, question navigator, trạng thái đã trả lời, Previous/Next và auto-submit. | Trải nghiệm làm bài rõ ràng, có thể quay lại câu bất kỳ. |
| 24/07 | Triển khai scoring, deadline, attempt limit, điều kiện eligible và cấp certificate idempotent; tạo trang Print/Save PDF. | Chứng chỉ chỉ được cấp sau khi hoàn tất lesson và thi đạt. |

## Kết quả đạt được

- Backend kiểm soát thời gian và số lần thi, không phụ thuộc đồng hồ trình duyệt.
- Hỗ trợ tối đa 12 option và nhiều đáp án đúng.
- Navigator hiển thị câu đã làm và cho phép xem lại.
- Certificate có tên Student, khóa học, ngày cấp và chức năng in PDF.

## Minh chứng từ dự án

- [Assessment service](https://github.com/Funacius/EduCloud/blob/main/backend/app/services/assessment_service.py)
- [Assessment editor](https://github.com/Funacius/EduCloud/blob/main/frontend/src/components/AssessmentEditor.tsx)
- [Student assessment page](https://github.com/Funacius/EduCloud/blob/main/frontend/src/pages/AssessmentPage.tsx)
- [Certificate page](https://github.com/Funacius/EduCloud/blob/main/frontend/src/pages/CertificatePage.tsx)
