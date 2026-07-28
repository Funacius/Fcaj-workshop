---
title: "Kiểm tra và xử lý lỗi"
weight: 8
chapter: false
pre: "<b>5.8.</b>"
---

# Kiểm tra và xử lý lỗi

Kiểm tra lần lượt: đăng nhập Cognito, phân quyền, tạo khóa học miễn phí, upload
thumbnail, multipart video và tài liệu, tạo final assessment, publish, học bằng
Student, gửi review, mở chứng chỉ, gửi Instructor application và duyệt bằng
Admin. Cuối cùng kiểm tra Admin Health/Logs, S3 inventory, Cost Explorer và
CloudWatch. Sau mỗi bước cần xác nhận Elastic Beanstalk vẫn Green.

Mở lại lesson, bấm **X** để gỡ PDF/video rồi Save; xác nhận URL trong lesson và
object S3 cũ đều đã mất. Thử xóa lesson đã có progress để xác nhận không còn lỗi
khóa ngoại từ database.

![EduCloud Lite live application](/images/workshop/09-live-application.png)

| Lỗi | Nơi cần kiểm tra |
| --- | --- |
| `Failed to fetch` | CloudFront origin, HTTPS, health và CORS |
| Refresh route bị 404 | Amplify rewrite `/index.html` |
| Cognito không đăng nhập | Pool ID, Client ID, trạng thái và password challenge |
| Parameter AccessDenied | EC2 role, Region và ARN |
| S3 403 | OAC, bucket policy, path behavior và object key |
| Deploy EB thất bại | Root của ZIP, Procfile, dependencies và log |
| EB Green nhưng API trả 502 | Kiểm tra `main.py`, `Procfile`, `app/` nằm ngay root ZIP và đọc `web.stdout.log` |
| Admin Logs báo ResourceNotFound | Copy đúng tên log group có phân biệt chữ hoa/thường từ CloudWatch Logs |
| Admin Logs refresh nhưng event mới nhất không đổi | Bật EB instance log streaming và kiểm tra stream có event mới; refresh không tự tạo log |
| Xóa lesson đã hoàn thành bị lỗi | Deploy backend mới để xóa progress phụ thuộc trước khi xóa lesson |
| Database lỗi | URL-encode, pooler host, TLS và password |

{{% notice tip %}}
Chỉ thay đổi một lớp mỗi lần rồi kiểm tra lại để xác định đúng nguyên nhân.
{{% /notice %}}
