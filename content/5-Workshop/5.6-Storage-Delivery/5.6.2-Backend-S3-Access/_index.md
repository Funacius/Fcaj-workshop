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

The backend creates short-lived multipart presigned URLs and the browser uploads
video parts directly to S3. `s3:ListBucket` uses the bucket ARN without `/*` and
is restricted to `courses/`; object actions use `courses/*`. `DeleteObject` is
required because removing or replacing a lesson video/material also removes the
previous object instead of leaving orphaned course data in the bucket.
