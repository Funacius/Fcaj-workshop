---
title: "Private Course Media Delivery with Amazon S3 and CloudFront"
menuTitle: "S3 and CloudFront Media"
weight: 2
pre: "<b>3.2.</b>"
---

## Private Course Media Delivery with Amazon S3 and CloudFront

Hello AWS Study Group VN! While deploying EduCloud Lite, one of the most
important problems I had to solve was course media delivery. In a learning
platform, thumbnails, documents, and videos should load quickly for students,
but the storage bucket should not be opened directly to the public.

This blog summarizes how I designed private course media storage with Amazon S3
and delivered it through Amazon CloudFront using a separate cache behavior for
course assets.

## 1. Problem Context

EduCloud Lite allows instructors to create courses and attach learning
resources. These resources include:

- Course thumbnails.
- Lesson videos.
- Downloadable materials.
- Images used in course content.

At first, using a plain public file URL seems simple. However, this creates
several risks:

- Anyone with the S3 object URL can access the file directly.
- Bucket policies may accidentally expose more files than intended.
- Video and image delivery can be slower without edge caching.
- It becomes harder to control CORS and HTTP headers consistently.

For an internship project, the goal was not to build a full enterprise media
platform. The goal was to demonstrate a safe and practical architecture: files
are stored privately in S3 and served through CloudFront.

## 2. Target Architecture

The final media delivery flow is:

1. The instructor uploads a thumbnail or course file from the EduCloud frontend.
2. The FastAPI backend validates the request and file type.
3. The backend stores the object in the EduCloud upload bucket.
4. The frontend stores or receives a CloudFront URL for the asset.
5. Students load course media through CloudFront under the `/courses/*` path.
6. CloudFront retrieves the object from S3 using Origin Access Control.

| Layer | AWS service / component | Responsibility |
| --- | --- | --- |
| Upload API | FastAPI on Elastic Beanstalk | Validate uploads and object keys |
| Object storage | Amazon S3 | Store course media privately |
| Delivery layer | Amazon CloudFront | Serve assets over HTTPS and cache static objects |
| Access control | Origin Access Control and bucket policy | Allow only CloudFront to read S3 objects |
| Runtime permission | IAM instance profile | Allow backend to upload/read scoped S3 keys |

The key idea is that S3 is not treated as a public website. It is treated as
private storage behind a controlled distribution layer.

## 3. S3 Bucket Design

The upload bucket was created separately from the Elastic Beanstalk service
bucket. This matters because the Beanstalk bucket is for deployment artifacts,
not user course content.

For EduCloud Lite, the upload bucket uses these settings:

- **General purpose bucket** in `ap-southeast-1`.
- **Block Public Access enabled**.
- **ACLs disabled** with bucket-owner enforced ownership.
- **Server-side encryption with SSE-S3**.
- Object keys grouped by application paths such as `courses/...`.

Keeping Block Public Access enabled was intentional. If the bucket needs public
access to work, the design is probably using the wrong access model.

## 4. CloudFront Origin and Cache Behavior

CloudFront was configured with two different origins:

| Origin | Purpose |
| --- | --- |
| Elastic Beanstalk origin | Dynamic backend API requests |
| S3 origin | Static course media files |

The media behavior uses the path pattern:

```text
courses/*
```

For this behavior, the origin points to the S3 bucket. The allowed HTTP methods
can stay simple:

```text
GET, HEAD
```

The cache policy can be optimized for static content because thumbnails and
course resources do not change on every request. This improves perceived
loading speed and reduces direct traffic to S3.

## 5. Origin Access Control

The important security piece is Origin Access Control. With OAC, CloudFront is
allowed to access the private S3 bucket, but normal internet users cannot access
the bucket directly.

The bucket policy should be scoped to:

- The S3 bucket and object path.
- The CloudFront service principal.
- The specific CloudFront distribution ARN.

This means a request is allowed only when it comes through the intended
CloudFront distribution. In EduCloud Lite, this prevented the need to disable
Block Public Access.

## 6. Backend Upload Rules

The backend still has responsibilities even when S3 and CloudFront are used.
EduCloud Lite should not accept arbitrary files blindly.

Important backend checks include:

- Validate file size.
- Validate content type.
- Generate safe object keys.
- Keep object paths predictable, for example under `courses/{course_id}/...`.
- Return a CloudFront-facing URL instead of exposing raw S3 URLs.

This keeps storage organized and avoids accidental overwrites or unsafe file
names.

## 7. CORS and Browser Loading

Because the frontend, API, and media URLs are served from different domains,
CORS must be handled carefully.

The final setup separates concerns:

- API CORS is handled by FastAPI.
- Static media delivery is handled by CloudFront/S3 response behavior.
- The frontend uses the CloudFront URL when rendering images and course assets.

When a thumbnail did not load during testing, the issue was not always the
frontend code. It could be:

- Wrong object key.
- Object was not uploaded.
- Missing S3 bucket policy for OAC.
- CloudFront behavior pattern did not match the URL.
- Browser cached an old failed response.

Debugging one layer at a time made the problem easier to isolate.

## 8. Practical Test Scenarios

| Test | Expected result |
| --- | --- |
| Open the S3 object URL directly | Access should be denied |
| Open the CloudFront `/courses/...` URL | Asset should load |
| Upload a thumbnail from Instructor UI | Backend stores file and course page displays it |
| Refresh the course detail page | Thumbnail still loads through CloudFront |
| Upload an unsupported file type | Backend rejects the file |

This combination proves that the bucket is private while the application still
serves media correctly.

## 9. Key Learnings

- **Private by default is safer:** Keeping S3 Block Public Access enabled avoids
  accidental exposure.
- **CloudFront is more than caching:** It becomes the controlled public delivery
  layer for private storage.
- **Behaviors matter:** API traffic and static media should not use the same
  cache settings.
- **Backend validation is still required:** Cloud storage does not replace file
  validation and authorization logic.
- **Debugging needs layers:** S3 permissions, CloudFront OAC, object keys, CORS,
  and browser cache can all cause similar-looking image load failures.

## Conclusion

Using Amazon S3 with CloudFront allowed EduCloud Lite to support course media
without making the storage bucket public. This design is small enough for an
internship project but still follows an important cloud principle: expose only
the delivery layer, not the storage layer.

For future improvement, the project can add signed URLs or signed cookies for
premium/private courses, lifecycle rules for old files, and CloudFront
invalidation automation when instructors replace course media.

**Source:** EduCloud Lite project repository and deployment report.  
**Repository:** [https://github.com/Funacius/EduCloud](https://github.com/Funacius/EduCloud)  
**Live application:** [https://main.djk00b5qbck73.amplifyapp.com/](https://main.djk00b5qbck73.amplifyapp.com/)
