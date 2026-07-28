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
        "s3:DeleteObject",
        "s3:AbortMultipartUpload",
        "s3:ListMultipartUploadParts"
      ],
      "Resource": "arn:aws:s3:::YOUR_UPLOAD_BUCKET/courses/*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::YOUR_UPLOAD_BUCKET",
      "Condition": {
        "StringLike": {
          "s3:prefix": ["courses/", "courses/*"]
        }
      }
    }
  ]
}
```

Backend tạo multipart presigned URL có thời hạn ngắn, sau đó trình duyệt upload
từng phần video trực tiếp lên S3. `s3:ListBucket` dùng ARN bucket không có `/*`
và bị giới hạn ở `courses/`; object action dùng `courses/*`. Quyền
`DeleteObject` cho phép xóa object cũ khi Instructor gỡ hoặc thay video/tài
liệu, nhờ đó không để lại dữ liệu mồ côi trong bucket.
