---
title: "Chia sẻ và góp ý"
weight: 7
chapter: false
pre: "<b>7.</b>"
---

# Chia sẻ và góp ý

Phần này tổng hợp cảm nhận cá nhân của tôi sau khi tham gia chương trình thực
tập First Cloud AI Journey và hoàn thành project EduCloud Lite.

## Đánh giá tổng quan

**1. Môi trường làm việc**

Môi trường thực tập khuyến khích tự học và phát triển project thực tế. Tôi có
không gian để làm việc độc lập, nghiên cứu dịch vụ AWS, thử các phương án
deployment và từng bước chuyển ứng dụng local thành website public. Mặc dù tôi
không giao tiếp với team quá thường xuyên, cấu trúc chương trình, template báo
cáo mẫu và định hướng học tập đã giúp tôi có lộ trình rõ ràng để thực hiện.

**2. Hỗ trợ từ mentor / định hướng chương trình**

Các hướng dẫn có sẵn giúp tôi hiểu cần nộp những gì và nên trình bày project như
thế nào. Phần hỗ trợ hữu ích nhất là cấu trúc report và workshop template, vì nó
giúp tôi tổ chức project thành các phần: worklog, proposal, blogs, events,
workshop, self-assessment và feedback. Nhờ đó bài nộp cuối cùng rõ ràng và
chuyên nghiệp hơn.

**3. Mức độ phù hợp với chuyên ngành**

Project EduCloud Lite liên quan trực tiếp đến chuyên ngành Khoa học máy tính của
tôi. Project yêu cầu thiết kế backend API, phát triển frontend, mô hình hóa
database, xác thực người dùng, deployment, debug và viết tài liệu phần mềm. Các
công việc này giúp tôi kết nối kiến thức học ở trường với các ràng buộc thực tế
khi triển khai như CORS, IAM permissions, environment variables, cloud storage và
production health checks.

**4. Cơ hội học tập và phát triển kỹ năng**

Trong kỳ thực tập, tôi học cách nhiều dịch vụ AWS phối hợp với nhau trong một hệ
thống full-stack. Tôi thực hành deploy FastAPI bằng Elastic Beanstalk, host React
bằng Amplify, quản lý xác thực bằng Cognito, lưu secret bằng Parameter Store,
phân phối file private bằng S3 và CloudFront, và kết nối backend với Supabase
PostgreSQL. Tôi cũng cải thiện khả năng xử lý lỗi chỉ xuất hiện ở production,
thay vì chỉ kiểm thử ở local.

**5. Văn hóa chương trình và tinh thần học tập**

First Cloud AI Journey khuyến khích học bằng cách xây dựng sản phẩm thật. Thay
vì chỉ học lý thuyết AWS, tôi phải tạo một website hoạt động, viết tài liệu kiến
trúc, giải thích lựa chọn thiết kế và chuẩn bị minh chứng triển khai. Điều này
làm quá trình học thực tế hơn và giúp tôi hiểu rõ sự khác biệt giữa một local
prototype và một cloud application có thể deploy.

**6. Chính sách / lợi ích của kỳ thực tập**

Lợi ích lớn nhất của kỳ thực tập là cơ hội làm một project kết hợp giữa software
development và cloud deployment. Quá trình học linh hoạt cho phép tôi dành nhiều
thời gian hơn cho các phần khó như authentication, CloudFront/S3 access, Elastic
Beanstalk configuration và Hugo documentation.

---

## Câu hỏi bổ sung

**Điều tôi cảm thấy hài lòng nhất trong kỳ thực tập là gì?**

Điều làm tôi hài lòng nhất là khi EduCloud Lite có thể truy cập bằng Amplify URL
public và các luồng login, course, instructor, profile, upload, assessment và
certificate hoạt động cùng nhau. Điều này cho thấy project không còn chỉ là demo
local, mà đã trở thành một cloud-hosted application hoàn chỉnh hơn.

**Theo tôi chương trình nên cải thiện điều gì cho các bạn thực tập sinh sau?**

Sẽ hữu ích nếu chương trình cung cấp checklist deployment ngắn ngay từ đầu, đặc
biệt cho IAM roles, Parameter Store, CORS, CloudFront behaviors, S3 bucket
policies, Cognito configuration và cleanup. Đây là những phần sinh viên mới học
AWS rất dễ mất nhiều thời gian để debug.

**Tôi có giới thiệu kỳ thực tập này cho bạn bè không?**

Có. Tôi sẽ giới thiệu chương trình này cho các bạn muốn hiểu cách một project
phần mềm được triển khai và vận hành trên cloud infrastructure. Chương trình đặc
biệt phù hợp với sinh viên đã biết web development cơ bản và muốn học cách đưa
ứng dụng của mình lên public một cách có cấu trúc và an toàn hơn.

---

## Góp ý và kỳ vọng

- Cung cấp một reference architecture tối thiểu ở đầu kỳ thực tập để sinh viên
  hiểu sớm mô hình deployment cần hướng tới.
- Khuyến khích sinh viên chụp screenshot và viết worklog theo từng tuần, thay vì
  đợi gần cuối kỳ mới gom lại minh chứng.
- Thêm hướng dẫn ngắn về ước lượng chi phí AWS và cleanup tài nguyên sau khi nộp
  bài.
- Bổ sung ví dụ troubleshooting thường gặp cho Cognito login, CORS, CloudFront
  origin behavior, S3 OAC access và Elastic Beanstalk deployment logs.
- Tiếp tục cho phép sinh viên chọn ý tưởng project riêng, vì làm một ứng dụng có
  ý nghĩa cá nhân giúp quá trình học hứng thú và chủ động hơn.

