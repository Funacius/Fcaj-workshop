---
title: "Kiểm tra và xử lý lỗi"
weight: 8
chapter: false
pre: "<b>5.8.</b>"
---

# Kiểm tra và xử lý lỗi

Kiểm tra lần lượt: đăng nhập Cognito, phân quyền, tạo khóa học, upload thumbnail,
video và tài liệu, tạo final assessment, publish, học bằng Student và mở chứng
chỉ. Sau mỗi bước cần xác nhận Elastic Beanstalk vẫn Green.

![EduCloud Lite live application](/images/workshop/09-live-application.png)

| Lỗi | Nơi cần kiểm tra |
| --- | --- |
| `Failed to fetch` | CloudFront origin, HTTPS, health và CORS |
| Refresh route bị 404 | Amplify rewrite `/index.html` |
| Cognito không đăng nhập | Pool ID, Client ID, trạng thái và password challenge |
| Parameter AccessDenied | EC2 role, Region và ARN |
| S3 403 | OAC, bucket policy, path behavior và object key |
| Deploy EB thất bại | Root của ZIP, Procfile, dependencies và log |
| Database lỗi | URL-encode, pooler host, TLS và password |

{{% notice tip %}}
Chỉ thay đổi một lớp mỗi lần rồi kiểm tra lại để xác định đúng nguyên nhân.
{{% /notice %}}
