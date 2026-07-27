---
title: "Cấp quyền S3 cho backend"
weight: 2
chapter: false
pre: "<b>5.6.2.</b>"
---

# Cấp quyền S3 cho backend

Attach least-privilege policy vào Elastic Beanstalk EC2 instance role:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::YOUR_UPLOAD_BUCKET/courses/*"
    }
  ]
}
```

Backend chỉ cần object access dưới prefix `courses/`. Giữ scope hẹp giúp giảm
rủi ro nếu có bug hoặc vấn đề credential.

