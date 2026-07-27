---
title: "OAC bucket policy"
weight: 4
chapter: false
pre: "<b>5.6.4.</b>"
---

# Thêm OAC bucket policy

Với S3 origin, copy bucket policy do CloudFront OAC tạo và dán vào S3 bucket
policy. Vẫn giữ **Block Public Access** bật.

![S3 bucket policy cho CloudFront OAC](/images/workshop/07b-s3-oac-bucket-policy.png)

Sau đó cập nhật backend variables:

```text
UPLOAD_STORAGE=s3
AWS_S3_BUCKET_NAME=YOUR_UPLOAD_BUCKET
AWS_S3_PUBLIC_BASE_URL=https://YOUR_DISTRIBUTION.cloudfront.net
PUBLIC_BASE_URL=https://YOUR_DISTRIBUTION.cloudfront.net
```

Apply Elastic Beanstalk environment update và chờ health trở về **Green**.
