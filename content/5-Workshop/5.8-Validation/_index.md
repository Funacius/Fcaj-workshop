---
title: "Validation and Troubleshooting"
weight: 8
chapter: false
pre: "<b>5.8.</b>"
---

# Validation and Troubleshooting

## Smoke test

1. Open the Amplify URL in a private browser window.
2. Register or sign in through Cognito.
3. Confirm Profile, Courses, Instructor, and Admin guards behave correctly.
4. Create a draft course as an Instructor.
5. Upload a thumbnail, video, and material; verify the returned URL uses CloudFront.
   Confirm the video uses multipart presigned upload and no duplicate thumbnail
   object is created when the same image is saved repeatedly.
6. Add lessons and a published final assessment.
   Reopen a lesson, remove its PDF/video with **X**, save, and confirm both the
   lesson URL and old S3 object are gone. Delete a completed lesson and confirm
   dependent progress does not cause a database error.
7. Publish the course.
8. Enroll as a Student, complete lessons, pass the assessment, and open the
   certificate.
9. Confirm Elastic Beanstalk remains green.
10. Submit an Instructor application as a Student and approve/reject it as Admin.
11. Submit a verified course rating/review and confirm the aggregate updates.
12. Open Admin Health and Logs; verify S3 inventory, Cost Explorer status, and
    recent CloudWatch events. Courses must not expose price or checkout controls.

![EduCloud Lite live application](/images/workshop/09-live-application.png)

## Endpoint checks

```powershell
Invoke-WebRequest "https://YOUR_CLOUDFRONT_DOMAIN/"
Invoke-WebRequest "https://YOUR_CLOUDFRONT_DOMAIN/docs"
Invoke-WebRequest "https://YOUR_AMPLIFY_DOMAIN/login"
```

## Common issues

| Symptom | Check |
| --- | --- |
| `Failed to fetch` | CloudFront origin, HTTPS, backend health, and CORS |
| SPA route returns 404 | Amplify rewrite to `/index.html` |
| Cognito user cannot sign in | User Pool/client IDs, status, email verification, password challenge |
| Parameter access denied | EC2 instance profile policy, Region, and ARN |
| S3 returns 403 | OAC selection, bucket policy, behavior path, object key |
| Thumbnail editor cannot load image | CloudFront URL, object content type, response CORS headers |
| Elastic Beanstalk deployment fails | ZIP root, `Procfile`, dependencies, and logs |
| EB is green but API returns 502 | Confirm `main.py`, `Procfile`, and `app/` are at the ZIP root; inspect `web.stdout.log` |
| Admin Logs says ResourceNotFound | Copy the exact case-sensitive CloudWatch log group name from CloudWatch Logs |
| Admin Logs polls but its newest event time is unchanged | Enable EB instance log streaming and verify the chosen stream receives new events; a refresh does not create an event |
| Deleting a completed lesson fails | Deploy the current backend so dependent progress is removed before the lesson |
| Database connection fails | URL encoding, pooler host, TLS, password, Supabase network status |

{{% notice tip %}}
Change one layer at a time and retest. Do not simultaneously change Cognito,
CloudFront, CORS, and application code, because the failing layer becomes harder
to identify.
{{% /notice %}}
