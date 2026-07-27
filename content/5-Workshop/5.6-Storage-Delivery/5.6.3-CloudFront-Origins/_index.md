---
title: "CloudFront Origins"
weight: 3
chapter: false
pre: "<b>5.6.3.</b>"
---

# Configure CloudFront Origins

Create one CloudFront distribution with two origins:

- Elastic Beanstalk custom origin for the default/API behavior.
- S3 origin with a new Origin Access Control.

Use these behaviors:

| Path | Origin | Methods | Cache |
| --- | --- | --- | --- |
| Default `*` | Elastic Beanstalk | All methods | `CachingDisabled` |
| `courses/*` | Private S3 | GET, HEAD | `CachingOptimized` |

![CloudFront origins and behaviors](/images/workshop/07-cloudfront-origins-behaviors.png)

