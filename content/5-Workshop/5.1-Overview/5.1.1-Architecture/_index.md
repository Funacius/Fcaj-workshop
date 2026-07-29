---
title: "Architecture"
weight: 1
chapter: false
pre: "<b>5.1.1.</b>"
---

# Architecture

![EduCloud AWS production architecture](/images/educloud-aws-architecture.png)

EduCloud Lite uses a small managed AWS architecture:

- **Frontend:** React, TypeScript, and Vite are hosted on AWS Amplify and
  automatically deployed from the GitHub `main` branch.
- **Backend:** FastAPI runs on Amazon EC2 through Elastic Beanstalk and provides
  APIs for courses, lessons, enrollment, progress, assessments, reviews,
  certificates, uploads, and administration.
- **Authentication:** Amazon Cognito handles registration, email verification,
  sign-in, and password recovery. The backend validates Cognito tokens and maps
  users to roles stored in Supabase.
- **Database:** Supabase PostgreSQL stores users, roles, courses, lessons,
  enrollments, progress, assessments, certificates, reviews, and Instructor
  applications.
- **Storage and delivery:** Amazon S3 privately stores thumbnails, videos, and
  documents. Videos are uploaded directly using presigned URLs, while CloudFront
  delivers course assets and routes `/api/*` traffic to Elastic Beanstalk.
- **Monitoring:** Elastic Beanstalk streams application logs to Amazon
  CloudWatch. The Admin dashboard displays recent logs, application health, S3
  usage, database statistics, and available AWS cost information.
- **Secrets and permissions:** AWS Systems Manager Parameter Store securely
  stores production secrets. Elastic Beanstalk references the Parameter Store
  ARNs, and IAM roles provide only the required access to S3, CloudWatch, Cost
  Explorer, and application configuration.
- **Scope:** All courses are free. Pricing, checkout, and payment processing
  are not included in the current system.

Elastic Beanstalk is configured as a single-instance environment to control cost
for the internship submission. It is a practical deployment target, not a
high-availability multi-AZ production design.

EduCloud offers free courses only. Pricing, checkout, and payment services are intentionally excluded from this architecture.

[Download the editable architecture diagram](/files/educloud-aws-architecture.drawio)
