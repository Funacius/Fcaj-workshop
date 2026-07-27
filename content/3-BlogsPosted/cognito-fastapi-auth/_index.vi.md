---
title: "Thiết kế xác thực với Amazon Cognito và FastAPI"
menuTitle: "Cognito và FastAPI"
weight: 3
pre: "<b>3.3.</b>"
---

## Thiết kế xác thực với Amazon Cognito và FastAPI

Khi EduCloud Lite chuyển từ môi trường local sang production, authentication là
một trong những phần thay đổi nhiều nhất. Tài khoản demo local rất tiện trong
giai đoạn phát triển, nhưng một website public cần luồng định danh an toàn hơn
cho mật khẩu, khôi phục tài khoản và người dùng đăng nhập lần đầu.

Bài viết này tóm tắt cách EduCloud Lite dùng Amazon Cognito để quản lý identity,
trong khi FastAPI và PostgreSQL vẫn kiểm soát role ứng dụng như Student,
Instructor và Admin.

## 1. Vì sao cần Cognito

Ở giai đoạn đầu, EduCloud Lite có legacy local authentication để test nhanh các
vai trò Student, Instructor và Admin.

Tuy nhiên, legacy authentication không phù hợp cho production vì:

- Không nên tự xử lý password một cách đơn giản trong application code.
- Luồng quên mật khẩu qua email khá khó làm an toàn nếu tự xây từ đầu.
- Tài khoản được cấp trước cần có bước đặt mật khẩu lần đầu rõ ràng.
- Public login không nên làm lộ email nào có tồn tại trong hệ thống.
- Authentication nên tách khỏi business role management.

Amazon Cognito User Pools giải quyết phần identity, còn EduCloud Lite vẫn giữ
authorization trong backend.

## 2. Authentication khác authorization

Một bài học quan trọng là không nên trộn authentication và authorization.

| Câu hỏi | Hệ thống chịu trách nhiệm | Ví dụ |
| --- | --- | --- |
| Người dùng này là ai? | Amazon Cognito | Email, mật khẩu, user pool subject |
| Token đăng nhập có hợp lệ không? | FastAPI backend | Verify chữ ký và claim của Cognito JWT |
| Người này được làm gì? | Database EduCloud | Role Student, Instructor, Admin |
| Người này được truy cập dữ liệu nào? | Backend services | Khóa học của mình, khóa đã đăng ký, admin dashboard |

Cognito xác định danh tính. EduCloud quyết định vai trò và quyền trong nền tảng
học tập.

Thiết kế này giúp xử lý một vấn đề thực tế: user có thể đã Confirmed trong
Cognito nhưng vẫn sai role trong EduCloud nếu record ở database chưa đúng.
Backend phải xem role trong database là nguồn dữ liệu chính.

## 3. Luồng đăng nhập production

Luồng đăng nhập production:

1. Người dùng nhập email và password trong React frontend.
2. Frontend authenticate với Cognito.
3. Cognito trả về identity token khi đăng nhập thành công.
4. Frontend gửi Cognito ID token tới FastAPI backend.
5. Backend verify chữ ký token và kiểm tra claim của user pool.
6. Backend ánh xạ Cognito `sub` với user record trong EduCloud.
7. Backend trả về application session/JWT có role của EduCloud.

Frontend không được tự quyết định ai là Instructor hoặc Admin. Frontend chỉ hiển
thị quyền mà backend trả về sau khi verify.

## 4. Luồng đặt mật khẩu lần đầu

EduCloud Lite hỗ trợ tài khoản được cấp sẵn. Cách này hữu ích khi admin muốn tạo
tài khoản instructor hoặc tài khoản demo trước khi người dùng đăng nhập.

Luồng xử lý:

1. Admin tạo Cognito user với mật khẩu tạm thời.
2. Người dùng đăng nhập lần đầu.
3. Cognito trả về challenge `NEW_PASSWORD_REQUIRED`.
4. Frontend hiển thị trang "Set your permanent password".
5. Người dùng nhập và xác nhận mật khẩu mới.
6. Frontend hoàn tất challenge với Cognito.
7. Sau khi đăng nhập thành công, EduCloud kiểm tra hồ sơ đã đủ chưa và điều
   hướng người dùng tới trang phù hợp.

Luồng này an toàn hơn việc chia sẻ mật khẩu vĩnh viễn ngay từ đầu.

## 5. Luồng quên mật khẩu

Với forgot password, Cognito xử lý việc gửi code qua email. EduCloud không cần
tự lưu reset token trong database ứng dụng.

Luồng xử lý:

1. Người dùng bấm "Forgot password?".
2. Người dùng nhập email.
3. Cognito gửi mã reset tới email.
4. Người dùng nhập mã và mật khẩu mới.
5. Cognito cập nhật mật khẩu.
6. Người dùng đăng nhập lại bằng mật khẩu mới.

Với project này, email là đủ. SMS không cần thiết, giúp giảm cấu hình và tránh
chi phí SNS không cần thiết.

## 6. Chiến lược email confirmation

EduCloud Lite có thể hỗ trợ nhiều kiểu tài khoản:

- User tự tạo có thể cần confirm email.
- User được admin tạo sẵn có thể được auto-confirm tùy cấu hình.
- Forgot password vẫn dùng email delivery dù việc kích hoạt tài khoản do workflow
  của project quyết định.

Cần phân biệt rõ: email confirmation chứng minh người dùng sở hữu email, còn
Instructor approval chứng minh người dùng được phép trở thành Instructor.

Ví dụ, một tài khoản có thể đã verify email nhưng vẫn là Student cho tới khi
Admin duyệt yêu cầu Instructor.

## 7. Kiểm tra bảo mật ở backend

FastAPI không được tin claim từ frontend một cách trực tiếp. Backend
authentication layer cần:

- Đọc public keys của Cognito.
- Verify chữ ký JWT.
- Kiểm tra issuer và audience/client.
- Lấy Cognito `sub`.
- Ánh xạ `sub` với user trong database.
- Chỉ trả về role được lưu trong EduCloud.

Điều này tránh lỗi phổ biến: tin role do browser gửi lên. Browser là client,
không phải nguồn quyền hạn.

## 8. Vấn đề đã gặp

| Vấn đề | Nguyên nhân | Cách xử lý |
| --- | --- | --- |
| Login hiện "legacy unauthenticated" | Frontend/backend vẫn dùng legacy mode hoặc env cũ | Cập nhật production env và tắt legacy auth |
| User đã confirmed nhưng chưa login được | Trạng thái email verification/account chưa đồng bộ | Điều chỉnh cấu hình Cognito và confirmation behavior |
| Tài khoản Instructor bị thành Student | Cognito identity đã có nhưng role trong database vẫn là Student | Sửa role trong database và giữ role ở server-side |
| Trang first-login báo thiếu profile | Tài khoản cấp sẵn chưa có đủ trường profile | Thêm luồng điền profile trước khi dùng tiếp |
| Amplify login báo `Failed to fetch` | API base URL, CORS hoặc CloudFront behavior sai | Cập nhật `VITE_API_BASE_URL`, backend CORS và behavior cho API |

## 9. Bài học rút ra

- **Cognito là identity, không phải toàn bộ application:** Cognito không thay thế
  role logic riêng của EduCloud.
- **Role trong database nên là nguồn chính:** Quyền Student, Instructor và Admin
  thuộc về business rule của EduCloud.
- **First-login flow giúp cấp tài khoản gọn hơn:** Tài khoản tạm có thể chuyển
  thành tài khoản thật mà không lộ mật khẩu vĩnh viễn.
- **Forgot password nên dùng managed service:** Cognito giúp tránh tự xây hệ
  thống reset-token.
- **Environment variables quyết định hành vi production:** Sai User Pool ID,
  Client ID hoặc legacy flag có thể làm frontend deploy chạy như local.

## 10. Hướng phát triển

- Chuyển sang HTTP-only cookie session để tăng an toàn khi lưu token trên trình
  duyệt.
- Thêm rate limiting và monitoring cho đăng nhập lỗi nhiều lần.
- Bổ sung công cụ admin để tạo và promote user rõ ràng hơn.
- Thêm end-to-end test cho login, first-login password setup và password
  recovery.
- Dùng infrastructure as code để tái tạo cấu hình Cognito ổn định hơn.

## Kết luận

Amazon Cognito giúp EduCloud Lite tiến gần hơn tới một web application
production thực tế. Cognito quản lý identity, password, first login và recovery,
trong khi FastAPI backend bảo vệ business rule của ứng dụng.

Bài học quan trọng nhất là tách trách nhiệm. Cognito trả lời câu hỏi "ai đang
đăng nhập?", còn backend EduCloud phải trả lời "người này được phép làm gì?"

**Nguồn:** Repository và báo cáo triển khai EduCloud Lite.  
**Repository:** [https://github.com/Funacius/EduCloud](https://github.com/Funacius/EduCloud)  
**Ứng dụng live:** [https://main.djk00b5qbck73.amplifyapp.com/](https://main.djk00b5qbck73.amplifyapp.com/)
