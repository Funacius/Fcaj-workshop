---
title: "Architecture"
weight: 1
chapter: false
pre: "<b>5.1.1.</b>"
---

# Architecture

![EduCloud AWS production architecture](/images/educloud-aws-architecture.png)

EduCloud Lite uses a small managed AWS architecture:

- **Amplify Hosting** serves the React/Vite single-page application.
- **Amazon Cognito** handles user authentication.
- **CloudFront** provides one HTTPS entry point for API and private course media.
- **Elastic Beanstalk** manages the application environment, with FastAPI running on its EC2 instance.
- **S3** stores private course thumbnails, videos, and learning materials.
- **CloudWatch** collects Elastic Beanstalk application logs and operational metrics.
- **Systems Manager Parameter Store** stores encrypted backend secrets.
- **Supabase PostgreSQL** stores application data over a TLS connection.

Elastic Beanstalk is configured as a single-instance environment to control cost
for the internship submission. It is a practical deployment target, not a
high-availability multi-AZ production design.

EduCloud offers free courses only. Pricing, checkout, and payment services are intentionally excluded from this architecture.

[Download the editable architecture diagram](/files/educloud-aws-architecture.drawio)
