---
title: "Week 11 - S3, CloudFront, and Amplify Hosting"
menuTitle: "Week 11"
weight: 11
pre: "<b>1.11.</b>"
---

**Period:** August 17, 2026 - August 21, 2026

## Objectives

- Prepare private S3 storage for course assets.
- Deliver assets and the backend through CloudFront over HTTPS.
- Deploy the React frontend automatically from GitHub.

## AWS Learning Phase

| Learning content | Reference | Application in EduCloud |
| --- | --- | --- |
| S3 private buckets, Block Public Access, server-side encryption, and access policies. | [Amazon S3 Security Best Practices](https://000069.awsstudygroup.com/) | Created a private uploads bucket for course assets. |
| CloudFront distributions, S3 origins, OAC/OAI-style private access, cache behaviors, and CORS. | [CloudFront with S3 Origin](https://000094.awsstudygroup.com/) | Delivered `courses/*` assets through CloudFront without making the bucket public. |
| Amplify Hosting builds from GitHub, environment variables, and SPA rewrites. | [Deploy Static Website on AWS](https://000137.awsstudygroup.com/) | Published the React frontend as an independent website link. |

## Work completed

| Date | Activity | Outcome |
| --- | --- | --- |
| Aug 17 | Created a Singapore upload bucket with blocked public access, disabled ACLs, and SSE-S3. | Kept objects private by default. |
| Aug 18 | Completed S3 abstraction, validation, storage mode, and bucket-scoped backend policy. | Preserved the local/S3 API contract. |
| Aug 19 | Added an S3 origin, OAC, bucket policy, `courses/*` behavior, caching, and CORS headers. | Allowed private CloudFront delivery. |
| Aug 20 | Configured the Beanstalk/API origin with disabled caching, AllViewer, and all API methods. | Exposed the API through HTTPS. |
| Aug 21 | Pushed GitHub main and configured Amplify monorepo root, build YAML, VITE variables, and SPA rewrite. | Automated frontend production builds. |

## Achievements

- Kept S3 private behind OAC and a runtime role.
- Validated thumbnail, material, and video upload limits.
- Separated cache behavior for assets and APIs.
- Deployed React from GitHub with client-route support.

## Project evidence

- [S3 service](https://github.com/Funacius/EduCloud/blob/main/backend/app/services/s3_service.py)
- [Upload routes](https://github.com/Funacius/EduCloud/blob/main/backend/app/routes/upload_routes.py)
- [Amplify configuration](https://github.com/Funacius/EduCloud/blob/main/amplify.yml)
- [AWS architecture](https://github.com/Funacius/EduCloud/tree/main/docs/architecture)
