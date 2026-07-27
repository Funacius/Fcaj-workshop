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
- **Elastic Beanstalk** runs the FastAPI backend.
- **S3** stores private course thumbnails, videos, and learning materials.
- **Systems Manager Parameter Store** stores encrypted backend secrets.
- **Supabase PostgreSQL** stores application data over a TLS connection.

Elastic Beanstalk is configured as a single-instance environment to control cost
for the internship submission. It is a practical deployment target, not a
high-availability multi-AZ production design.

[Download the editable architecture diagram](/files/educloud-aws-architecture.drawio)

