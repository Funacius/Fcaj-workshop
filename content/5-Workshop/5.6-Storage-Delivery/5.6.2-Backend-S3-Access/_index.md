---
title: "Backend S3 Access"
weight: 2
chapter: false
pre: "<b>5.6.2.</b>"
---

# Grant Backend S3 Access

Attach a least-privilege policy to the Elastic Beanstalk EC2 instance role:

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

The backend only needs object access under the `courses/` prefix. Keeping the
scope narrow reduces the blast radius if a bug or credential issue appears.

