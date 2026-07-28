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
| Database lỗi | URL-encode, pooler host, TLS và password |

{{% notice tip %}}
Chỉ thay đổi một lớp mỗi lần rồi kiểm tra lại để xác định đúng nguyên nhân.
{{% /notice %}}
