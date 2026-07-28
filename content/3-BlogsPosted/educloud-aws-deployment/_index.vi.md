---
title: "Bảo vệ tài nguyên khóa học riêng tư bằng Amazon S3 và CloudFront OAC"
menuTitle: "S3 private media với CloudFront"
weight: 1
pre: "<b>3.1.</b>"
---

# Bảo vệ tài nguyên khóa học riêng tư bằng Amazon S3 và CloudFront OAC

Một nền tảng học trực tuyến thường phải phân phối thumbnail, video và tài liệu
cho từng khóa học. Public S3 bucket thì dễ cấu hình, nhưng người dùng có thể bỏ
qua ứng dụng và truy cập trực tiếp bằng URL S3. Bài viết này trình bày một kiến
trúc nhỏ, có thể triển khai thực tế, giúp EduCloud Lite giữ file riêng tư nhưng
vẫn phân phối qua một endpoint HTTPS công khai.

Bài viết được tham khảo từ bài AWS Networking & Content Delivery Blog
[Amazon CloudFront introduces Origin Access Control (OAC)](https://aws.amazon.com/blogs/networking-and-content-delivery/amazon-cloudfront-introduces-origin-access-control-oac/).
Nội dung dưới đây là phần triển khai và phân tích riêng cho EduCloud Lite,
không sao chép nguyên văn bài nguồn.

## 1. Bài toán và mục tiêu thiết kế

EduCloud Lite có ba nhóm tài nguyên được upload:

- thumbnail khóa học;
- video bài học; và
- tài liệu đính kèm trong lesson.

Thiết kế cần đáp ứng bốn mục tiêu:

1. không cho đọc object bằng public S3 URL;
2. người dùng truy cập qua một domain HTTPS duy nhất;
3. tách riêng API traffic và media traffic; và
4. chi phí, cấu hình phù hợp với một dự án thực tập.

## 2. Kiến trúc

Luồng request như sau:

1. Trình duyệt tải React application từ Amplify Hosting.
2. Ứng dụng gọi CloudFront qua đường dẫn `/courses/*` để lấy media.
3. CloudFront ký origin request bằng Origin Access Control (OAC).
4. S3 xác thực chữ ký và chỉ trả object cho distribution đã được cấp quyền.

API dùng behavior riêng tại `/api/*` và được forward tới FastAPI trên Elastic
Beanstalk với caching bị tắt.

![Kiến trúc media riêng tư của EduCloud](/images/educloud-aws-architecture.png)

*Hình 1 — EduCloud Lite tách frontend delivery, API execution và private course
storage.*

CloudFront OAC được ưu tiên hơn Origin Access Identity (OAI) cũ vì hỗ trợ các
Region S3, SSE-KMS và nhiều HTTP method hơn. Xem
[bài công bố của AWS](https://aws.amazon.com/blogs/networking-and-content-delivery/amazon-cloudfront-introduces-origin-access-control-oac/)
và [tài liệu CloudFront OAC](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)
để hiểu access model chính thức.

## 3. Các bước triển khai

### 3.1 Tạo S3 bucket riêng tư

Bucket chỉ dùng cho file upload của EduCloud. Các thiết lập chính:

- bật Block all public access;
- Object Ownership: bucket owner enforced, tắt ACL;
- mã hóa mặc định bằng SSE-S3; và
- bật versioning/lifecycle khi cần lưu trữ dài hạn.

Tên bucket không được dùng làm public media URL. Backend upload object theo
prefix như `courses/<course-id>/` và lưu object key trong PostgreSQL.

### 3.2 Tạo Origin Access Control

Trong CloudFront, tạo OAC cho S3 origin và chọn **Sign requests**. Distribution
sẽ tạo request có chữ ký SigV4 để S3 xác thực. Vì vậy không cần mở public bucket.

Bucket policy cấp `s3:GetObject` cho CloudFront service principal và giới hạn
request theo ARN của EduCloud distribution. Đây là ranh giới bảo mật chính:
ngay cả khi biết tên bucket, người dùng vẫn không thể đọc object trực tiếp.

Policy do CloudFront sinh ra thường có dạng sau (thay các placeholder bằng giá
trị thật trong tài khoản AWS):

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "AllowCloudFrontServicePrincipalReadOnly",
    "Effect": "Allow",
    "Principal": { "Service": "cloudfront.amazonaws.com" },
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::EDUCLOUD_BUCKET/courses/*",
    "Condition": {
      "StringEquals": {
        "AWS:SourceArn": "arn:aws:cloudfront::ACCOUNT_ID:distribution/DISTRIBUTION_ID"
      }
    }
  }]
}
```

Đây là policy tham khảo, không phải credential và không nên dán nguyên trạng
vào production. Distribution ARN và prefix object phải khớp deployment thật.

### 3.3 Cấu hình CloudFront behaviors

| Path pattern | Origin | Cache policy | Allowed methods |
| --- | --- | --- | --- |
| `/courses/*` | S3 riêng tư có OAC | CachingOptimized | GET, HEAD |
| `/api/*` | Elastic Beanstalk | CachingDisabled | GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE |
| Default (`*`) | frontend/API fallback | tùy dự án | tùy dự án |

Behavior media dùng cache vì file khóa học ít thay đổi hơn API response. API
behavior tắt cache để không trả nhầm dữ liệu xác thực hoặc tiến độ cũ từ edge.

### 3.4 Kết nối upload với ứng dụng

Luồng upload của Instructor:

1. FastAPI xác thực Cognito access token và kiểm tra role Instructor.
2. FastAPI kiểm tra loại file và quyền sở hữu khóa học.
3. File được upload lên S3, object key được lưu cùng lesson/course record trong
   Supabase PostgreSQL.
4. API trả về CloudFront URL thay vì S3 URL.

Khi cần xử lý file lớn hơn, có thể mở rộng bằng presigned upload URL có thời hạn
ngắn để browser upload trực tiếp lên S3, không phải đi qua application server.

### 3.5 Kiểm tra ranh giới bảo mật

Các kiểm thử đã dùng cho EduCloud:

- mở CloudFront URL khi đã đăng nhập và xác nhận asset hiển thị;
- mở S3 URL tương ứng và xác nhận truy cập public bị từ chối;
- upload thumbnail bằng Instructor và kiểm tra object dưới đúng course prefix; và
- gọi API qua `/api/*`, xác nhận request tới Elastic Beanstalk thay vì S3 origin.

Khi gặp `403`, `404` hoặc lỗi CORS, nên kiểm tra CloudFront/S3 log và health
event của Elastic Beanstalk trước.

## 4. Bảo mật và vận hành

### Least privilege

EC2 instance profile của Elastic Beanstalk chỉ nhận các quyền SSM và S3 cần cho
ứng dụng. Browser chỉ chứa các biến `VITE_*` có thể công khai; database URL,
JWT secret và AWS credentials luôn ở phía server.

### Cache invalidation

Nếu thay object bằng cùng key, CloudFront có thể tiếp tục trả bản cache cũ cho
đến khi TTL hết hạn. Dùng object key có version, ví dụ
`thumbnail-<timestamp>.jpg`, sẽ giảm nhu cầu invalidation.

### Kiểm soát chi phí

Với môi trường demo, một Elastic Beanstalk single instance, một S3 bucket và
CloudFront pay-as-you-go là đủ. Nên theo dõi log retention và lượng tải video vì
data transfer và request volume có thể trở thành chi phí chính.

### Vì sao dùng S3 và CloudFront thay vì lưu media trên backend?

S3 là object storage bền vững, còn CloudFront được thiết kế để phân phối nội
dung từ edge. Tách media khỏi filesystem của Elastic Beanstalk giúp backend
không bị đầy ổ đĩa và cho phép API scale độc lập với lưu lượng video.

## 5. Kết quả trên EduCloud Lite

Deployment hiện tại sử dụng:

- Amplify Hosting cho React/Vite frontend;
- Elastic Beanstalk cho FastAPI API;
- Cognito cho sign-in và password recovery;
- Supabase PostgreSQL cho application data; và
- S3 kết hợp CloudFront OAC cho private course media.

Nhờ vậy mentor chỉ cần một public website, nhưng kiến trúc vẫn tách rõ identity,
business data, API execution và object storage.

## Kết luận

Bài học chính là không cần một media service phức tạp để bảo vệ file riêng tư.
Một S3 bucket bị khóa public, bucket policy giới hạn và CloudFront OAC đã đủ để
tạo đường phân phối an toàn cho LMS prototype. Sau này có thể mở rộng bằng
signed URL, direct-to-S3 upload, WAF rule và CloudWatch alarm.

## Tài liệu tham khảo

- [Amazon CloudFront introduces Origin Access Control (OAC) — AWS Networking & Content Delivery Blog](https://aws.amazon.com/blogs/networking-and-content-delivery/amazon-cloudfront-introduces-origin-access-control-oac/)
- [Restrict access to an Amazon S3 origin — Amazon CloudFront Developer Guide](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)
- [Restrict access to files — Amazon CloudFront Developer Guide](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-overview.html)
- [Mã nguồn EduCloud Lite](https://github.com/Funacius/EduCloud)
- [Ứng dụng EduCloud Lite](https://main.djk00b5qbck73.amplifyapp.com/)
