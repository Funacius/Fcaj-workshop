---
title: "CloudFront origins"
weight: 3
chapter: false
pre: "<b>5.6.3.</b>"
---

# Cấu hình CloudFront origins

Tạo một CloudFront distribution với hai origin:

- Elastic Beanstalk custom origin cho default/API behavior.
- S3 origin với Origin Access Control mới.

Cấu hình behavior:

| Path | Origin | Methods | Cache |
| --- | --- | --- | --- |
| Default `*` | Elastic Beanstalk | All methods | `CachingDisabled` |
| `courses/*` | Private S3 | GET, HEAD | `CachingOptimized` |

![CloudFront origins and behaviors](/images/workshop/07-cloudfront-origins-behaviors.png)

