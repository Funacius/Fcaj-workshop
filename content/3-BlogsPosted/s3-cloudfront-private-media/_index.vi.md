---
title: "Phân phối tài nguyên khóa học private với Amazon S3 và CloudFront"
menuTitle: "S3 và CloudFront"
weight: 2
pre: "<b>3.2.</b>"
---

## Phân phối tài nguyên khóa học private với Amazon S3 và CloudFront

Trong quá trình triển khai EduCloud Lite, một vấn đề quan trọng là cách lưu trữ
và phân phối tài nguyên khóa học. Thumbnail, tài liệu và video cần tải nhanh cho
người học, nhưng bucket lưu trữ không nên được mở public trực tiếp.

Bài viết này tóm tắt cách EduCloud Lite sử dụng Amazon S3 để lưu trữ private và
Amazon CloudFront để phân phối tài nguyên qua HTTPS.

## 1. Bối cảnh vấn đề

EduCloud Lite cho phép instructor tạo khóa học và thêm tài nguyên học tập, bao
gồm:

- Thumbnail khóa học.
- Video bài học.
- Tài liệu đính kèm.
- Hình ảnh dùng trong nội dung khóa học.

Cách đơn giản nhất là dùng URL public trực tiếp từ S3, nhưng cách này có nhiều
rủi ro:

- Ai có URL object đều có thể truy cập file trực tiếp.
- Bucket policy có thể vô tình mở quyền nhiều hơn cần thiết.
- Video và hình ảnh có thể tải chậm nếu không có caching ở edge.
- Việc kiểm soát CORS và HTTP header trở nên khó đồng bộ.

Vì vậy, mục tiêu của EduCloud Lite là giữ S3 private và chỉ cho phép truy cập
tài nguyên thông qua CloudFront.

## 2. Kiến trúc mục tiêu

Luồng phân phối tài nguyên cuối cùng:

1. Instructor upload thumbnail hoặc tài liệu từ frontend EduCloud.
2. Backend FastAPI kiểm tra request và loại file.
3. Backend lưu object vào bucket upload của EduCloud.
4. Frontend nhận hoặc lưu URL thông qua CloudFront.
5. Student tải tài nguyên khóa học qua CloudFront dưới path `/courses/*`.
6. CloudFront đọc object từ S3 bằng Origin Access Control.

| Tầng | Dịch vụ / thành phần | Trách nhiệm |
| --- | --- | --- |
| Upload API | FastAPI trên Elastic Beanstalk | Kiểm tra upload và object key |
| Object storage | Amazon S3 | Lưu tài nguyên khóa học ở chế độ private |
| Delivery layer | Amazon CloudFront | Phân phối file qua HTTPS và cache static asset |
| Access control | Origin Access Control và bucket policy | Chỉ cho CloudFront đọc object từ S3 |
| Runtime permission | IAM instance profile | Cho backend quyền upload/read đúng phạm vi |

Điểm quan trọng là S3 không được dùng như một public website, mà được dùng như
kho lưu trữ private phía sau CloudFront.

## 3. Thiết kế S3 bucket

Bucket upload được tạo riêng, không dùng chung với bucket mặc định của Elastic
Beanstalk. Bucket của Beanstalk dùng cho deployment artifact, không nên dùng để
lưu nội dung khóa học của người dùng.

Bucket upload của EduCloud Lite có các cấu hình chính:

- Bucket general purpose ở `ap-southeast-1`.
- Bật **Block Public Access**.
- Tắt ACL, dùng bucket-owner enforced ownership.
- Bật server-side encryption bằng SSE-S3.
- Gom object theo path ứng dụng, ví dụ `courses/...`.

Giữ Block Public Access là lựa chọn có chủ ý. Nếu hệ thống cần tắt Block Public
Access để file tải được, nghĩa là mô hình truy cập đang chưa đúng.

## 4. CloudFront origin và cache behavior

CloudFront được cấu hình với hai origin khác nhau:

| Origin | Mục đích |
| --- | --- |
| Elastic Beanstalk origin | Nhận request API động |
| S3 origin | Phân phối tài nguyên khóa học |

Behavior cho media dùng path pattern:

```text
courses/*
```

Với behavior này, origin trỏ tới S3 bucket. HTTP methods có thể giữ ở mức đơn
giản:

```text
GET, HEAD
```

Cache policy có thể tối ưu cho static content vì thumbnail và tài liệu khóa học
không thay đổi ở mỗi request. Điều này giúp giảm tải trực tiếp tới S3 và cải
thiện tốc độ tải trang.

## 5. Origin Access Control

Phần bảo mật quan trọng nhất là Origin Access Control. Với OAC, CloudFront có
quyền đọc object trong private S3 bucket, nhưng người dùng internet không thể
truy cập bucket trực tiếp.

Bucket policy cần giới hạn theo:

- Bucket và object path cần đọc.
- Service principal của CloudFront.
- ARN của đúng CloudFront distribution.

Nhờ đó, request chỉ được chấp nhận khi đi qua distribution đã cấu hình. EduCloud
Lite không cần tắt Block Public Access để phân phối tài nguyên.

## 6. Quy tắc upload ở backend

Ngay cả khi dùng S3 và CloudFront, backend vẫn phải kiểm soát upload. EduCloud
Lite không nên nhận mọi file một cách tự do.

Các kiểm tra quan trọng:

- Giới hạn dung lượng file.
- Kiểm tra content type.
- Tạo object key an toàn.
- Gom file theo path dễ quản lý, ví dụ `courses/{course_id}/...`.
- Trả về URL hướng tới CloudFront thay vì lộ URL S3 trực tiếp.

Điều này giúp storage gọn hơn và tránh lỗi ghi đè hoặc tên file không an toàn.

## 7. CORS và lỗi tải ảnh

Vì frontend, API và media URL có thể nằm ở các domain khác nhau, CORS cần được
xử lý cẩn thận.

Thiết kế cuối cùng tách rõ:

- API CORS do FastAPI xử lý.
- Static media do CloudFront/S3 response behavior xử lý.
- Frontend render ảnh và tài nguyên bằng CloudFront URL.

Khi thumbnail không tải được, nguyên nhân không nhất thiết nằm ở frontend. Một
số nguyên nhân thường gặp:

- Sai object key.
- File chưa upload thành công.
- Bucket policy chưa cấp quyền cho OAC.
- Path pattern của CloudFront không match URL.
- Trình duyệt cache lại response lỗi cũ.

Kiểm tra từng tầng giúp quá trình debug dễ kiểm soát hơn.

## 8. Kịch bản kiểm thử

| Kiểm thử | Kết quả mong đợi |
| --- | --- |
| Mở URL object S3 trực tiếp | Bị từ chối truy cập |
| Mở URL CloudFront `/courses/...` | Tài nguyên tải được |
| Upload thumbnail từ Instructor UI | Backend lưu file và trang course hiển thị ảnh |
| Refresh trang chi tiết khóa học | Thumbnail vẫn tải qua CloudFront |
| Upload file không hợp lệ | Backend từ chối file |

Các kịch bản này chứng minh bucket vẫn private nhưng ứng dụng vẫn phân phối
media bình thường.

## 9. Bài học rút ra

- **Private by default an toàn hơn:** Giữ Block Public Access giúp hạn chế rủi ro
  mở nhầm dữ liệu.
- **CloudFront không chỉ để cache:** CloudFront là lớp public có kiểm soát phía
  trước private storage.
- **Behavior rất quan trọng:** API và static media cần cache policy khác nhau.
- **Backend vẫn phải validate:** Cloud storage không thay thế được logic kiểm
  tra file và phân quyền.
- **Debug phải theo từng tầng:** S3 permission, CloudFront OAC, object key, CORS
  và browser cache đều có thể gây lỗi giống nhau.

## Kết luận

Kết hợp Amazon S3 và CloudFront giúp EduCloud Lite phân phối tài nguyên khóa học
mà không cần public bucket. Thiết kế này đủ gọn cho một project thực tập nhưng
vẫn thể hiện đúng nguyên tắc cloud: chỉ public lớp phân phối, không public lớp
lưu trữ.

Trong tương lai, hệ thống có thể bổ sung signed URL hoặc signed cookie cho khóa
học riêng tư, lifecycle rule cho file cũ và tự động invalidation CloudFront khi
instructor thay đổi tài nguyên.

**Nguồn:** Repository và báo cáo triển khai EduCloud Lite.  
**Repository:** [https://github.com/Funacius/EduCloud](https://github.com/Funacius/EduCloud)  
**Ứng dụng live:** [https://main.djk00b5qbck73.amplifyapp.com/](https://main.djk00b5qbck73.amplifyapp.com/)
