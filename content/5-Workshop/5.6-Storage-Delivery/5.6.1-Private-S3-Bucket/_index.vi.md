---
title: "Tạo S3 private bucket"
weight: 1
chapter: false
pre: "<b>5.6.1.</b>"
---

# Tạo S3 private bucket

Tạo upload bucket riêng:

- Bucket type: General purpose.
- Region: `ap-southeast-1`.
- Object Ownership: Bucket owner enforced.
- Block all public access: enabled.
- Default encryption: SSE-S3.
- Versioning: tùy chọn trong workshop.
- Object Lock: disabled.

Không dùng lại bucket hệ thống do Elastic Beanstalk tự tạo.

![S3 private bucket](/images/workshop/06-s3-private-bucket.png)

