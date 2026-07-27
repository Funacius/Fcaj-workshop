---
title: "Private S3 Bucket"
weight: 1
chapter: false
pre: "<b>5.6.1.</b>"
---

# Create a Private S3 Bucket

Create a separate upload bucket:

- Bucket type: General purpose.
- Region: `ap-southeast-1`.
- Object Ownership: Bucket owner enforced.
- Block all public access: enabled.
- Default encryption: SSE-S3.
- Versioning: optional for this workshop.
- Object Lock: disabled.

Do not reuse the automatically created Elastic Beanstalk service bucket.

![S3 private bucket](/images/workshop/06-s3-private-bucket.png)

