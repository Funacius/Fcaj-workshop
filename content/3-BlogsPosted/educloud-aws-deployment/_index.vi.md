---
title: "Triển khai EduCloud Lite trên AWS"
menuTitle: "Triển khai EduCloud"
weight: 1
pre: "<b>3.1.</b>"
---

## Triển khai EduCloud Lite trên AWS: Từ LMS local đến website public

Xin chào AWS Study Group VN! Trong kỳ thực tập First Cloud AI Journey, tôi xây
dựng **EduCloud Lite**, một nền tảng quản lý học tập gọn nhẹ hỗ trợ publish
khóa học, ghi danh sinh viên, theo dõi tiến độ bài học, làm final assessment và
cấp chứng chỉ hoàn thành.

Bài viết này tóm tắt cách tôi đưa dự án từ ứng dụng React/FastAPI chạy local
lên kiến trúc AWS có managed identity, private storage, HTTPS delivery và một
link frontend public để trình bày sản phẩm.

## 1. Bối cảnh và mục tiêu dự án

EduCloud Lite có ba vai trò chính:

- **Student:** Xem khóa học, ghi danh, học bài, hoàn thành assessment và xem
  certificate.
- **Instructor:** Tạo khóa học, upload thumbnail/tài nguyên, quản lý lesson,
  cấu hình final assessment và publish nội dung.
- **Admin:** Duyệt yêu cầu Instructor, quản lý user/course và theo dõi trạng
  thái hệ thống cơ bản.

Mục tiêu triển khai không phải là xây dựng một LMS enterprise lớn. Mục tiêu là
tạo một kiến trúc cloud hoạt động thật, dễ giải thích, kiểm soát chi phí và có
thể demo bằng một đường link độc lập.

## 2. Điểm kỹ thuật nổi bật

### Tách identity, application data và storage

Thiết kế đầu tiên là không gom mọi trách nhiệm vào một server.

| Trách nhiệm | Service / Component | Lý do sử dụng |
| --- | --- | --- |
| Browser application | React, TypeScript, Vite, Amplify Hosting | Build frontend nhanh và host public |
| Business API | FastAPI trên Elastic Beanstalk | Backend Python rõ ràng theo REST API |
| User identity | Amazon Cognito | Quản lý mật khẩu, xác nhận, first login và password recovery |
| Application data | Supabase PostgreSQL | Lưu user, course, lesson, progress, attempt và certificate |
| Private course assets | Amazon S3 | Lưu thumbnail, video và tài liệu |
| HTTPS delivery và routing | Amazon CloudFront | Một lớp phân phối cho API và private course assets |
| Secrets | Systems Manager Parameter Store | Đưa database URL và JWT secret ra khỏi source code |

Cách tách này giúp quá trình debug rõ ràng hơn. Khi login lỗi, tôi kiểm tra
Cognito và token exchange. Khi file không load, tôi kiểm tra S3, CloudFront
behavior, bucket policy và CORS. Khi API lỗi, tôi kiểm tra Elastic Beanstalk
health và backend logs.

### Luồng request production

Luồng request sau cùng:

1. Người dùng mở React frontend từ Amplify Hosting.
2. Browser đăng nhập bằng Amazon Cognito.
3. Frontend gửi API request đến CloudFront qua `/api/*`.
4. CloudFront forward API traffic đến Elastic Beanstalk.
5. FastAPI xác thực token, kiểm tra role và đọc/ghi Supabase PostgreSQL.
6. File khóa học được upload vào S3.
7. Tài nguyên khóa học được phân phối qua CloudFront ở `/courses/*` bằng private
   S3 access.

## 3. Các bước triển khai thực tế

### Deploy backend bằng Elastic Beanstalk

FastAPI được deploy lên Elastic Beanstalk với Python 3.12. Gói backend gồm mã
nguồn, dependencies và `Procfile` để khởi động API server.

Các cấu hình quan trọng không được lưu trực tiếp trong source code. Thay vào đó,
deployment dùng Parameter Store cho:

- `DATABASE_URL`
- `JWT_SECRET_KEY`

EC2 instance profile của Elastic Beanstalk chỉ được cấp quyền đọc đúng các
parameter cần thiết cho EduCloud Lite.

### Cognito authentication

Cognito được dùng làm identity provider, phụ trách:

- Đăng nhập bằng email.
- Forgot password và reset code.
- First-login password challenge cho tài khoản được cấp sẵn.
- Email verification tùy theo cấu hình tài khoản.

Ứng dụng vẫn lưu role trong PostgreSQL. Cognito trả lời câu hỏi "người này là
ai?", còn EduCloud trả lời "người này được làm gì trong LMS?"

### Private storage với S3 và CloudFront

Tài nguyên khóa học được lưu trong một S3 bucket riêng, không dùng bucket service
của Elastic Beanstalk. Upload bucket được cấu hình:

- Bật Block Public Access.
- Tắt ACL.
- Server-side encryption bằng SSE-S3.
- Bucket policy giới hạn cho CloudFront Origin Access Control.

CloudFront có các behavior tách riêng:

| Path pattern | Origin | Cache policy | Mục đích |
| --- | --- | --- | --- |
| `Default (*)` / `/api/*` | Elastic Beanstalk | Caching disabled | Dynamic API requests |
| `/courses/*` | Private S3 origin | Caching optimized | Thumbnail, video và tài liệu khóa học |

Cách này cho phép phân phối tài nguyên hiệu quả mà không cần public S3 bucket.

### Deploy frontend bằng Amplify Hosting

Frontend được deploy từ GitHub bằng Amplify Hosting. Monorepo app root là
`frontend`, với:

- Build command: `npm run build`
- Output directory: `dist`
- SPA rewrite về `/index.html`

Frontend build dùng các biến `VITE_*` an toàn để public trong browser, ví dụ:

- `VITE_API_BASE_URL`
- `VITE_COGNITO_REGION`
- `VITE_COGNITO_USER_POOL_ID`
- `VITE_COGNITO_CLIENT_ID`

Database URL, JWT secret và AWS credentials không bao giờ được đưa vào frontend
environment.

## 4. Vấn đề gặp phải khi triển khai

| Vấn đề | Nguyên nhân | Cách xử lý |
| --- | --- | --- |
| Backend lỗi khi startup | Thiếu dependency hoặc cấu hình production chưa đúng | Xem Elastic Beanstalk logs và sửa requirements/config |
| Cognito user login được nhưng sai role | Identity nằm trong Cognito, role nằm trong Supabase | Cập nhật user record trong PostgreSQL và giữ role assignment ở server-side |
| Amplify frontend báo `Failed to fetch` | CORS hoặc API origin chưa khớp | Cập nhật `CORS_ORIGINS` ở backend và `VITE_API_BASE_URL` ở frontend |
| S3 asset trả về 403 | CloudFront origin access hoặc bucket policy chưa đủ | Thêm OAC và generated bucket policy, vẫn giữ Block Public Access |
| Refresh `/login` hoặc `/profile` bị lỗi | React dùng client-side routing | Thêm Amplify SPA rewrite về `/index.html` |

## 5. AWS services trong kiến trúc

- **AWS Amplify Hosting:** Build và host React frontend từ GitHub.
- **Amazon CloudFront:** Route API traffic và phân phối private course assets.
- **AWS Elastic Beanstalk:** Chạy FastAPI backend.
- **Amazon Cognito:** Quản lý identity, sign-in, confirmation và password
  recovery.
- **Amazon S3:** Lưu thumbnail, video và tài liệu khóa học.
- **AWS Systems Manager Parameter Store:** Lưu secret production.
- **AWS IAM:** Cấp quyền runtime theo nguyên tắc least privilege.
- **Amazon CloudWatch:** Hỗ trợ xem log và kiểm tra health.

## 6. Bài học rút ra

- **Tách identity khỏi role ứng dụng:** Cognito quản lý login, còn database ứng
  dụng quyết định Student, Instructor và Admin access.
- **Không public S3 chỉ để file load được:** CloudFront OAC giúp phân phối file
  private mà vẫn giữ Block Public Access.
- **Frontend variables là public:** Chỉ đưa các giá trị `VITE_*` an toàn vào
  Amplify.
- **Debug production phải theo từng lớp:** API, CORS, Cognito, CloudFront, S3 và
  database cần được kiểm tra lần lượt.
- **Kiểm soát chi phí là một phần của thiết kế:** Single-instance backend,
  private S3 và logging vừa đủ là phù hợp cho bài nộp thực tập.

## 7. Hạn chế và hướng phát triển

- Thêm infrastructure as code để giảm thao tác thủ công trên AWS Console.
- Thêm end-to-end browser tests cho luồng Student, Instructor và Admin.
- Dùng Alembic để quản lý migration production tốt hơn.
- Thêm CloudWatch alarms và dashboard dùng chung.
- Hardening token storage và cookie/session strategy nếu mở rộng production.

## Kết luận

Triển khai EduCloud Lite trên AWS cho thấy một project sinh viên vẫn có thể đi
theo các nguyên tắc cloud architecture thực tế: managed identity, private
storage, least-privilege IAM, externalized secrets, HTTPS delivery và tài liệu
triển khai có thể làm lại.

Bài học quan trọng nhất là "chạy được local" mới chỉ là bước đầu. Phần kỹ thuật
thật sự bắt đầu khi authentication, networking, storage permissions, CORS,
logging và cost control phải hoạt động cùng nhau.

**Nguồn:** Repository và báo cáo triển khai EduCloud Lite.  
**Repository:** [https://github.com/Funacius/EduCloud](https://github.com/Funacius/EduCloud)  
**Live application:** [https://main.djk00b5qbck73.amplifyapp.com/](https://main.djk00b5qbck73.amplifyapp.com/)
