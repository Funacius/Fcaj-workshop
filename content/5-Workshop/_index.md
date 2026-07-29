---
title: "Workshop"
weight: 5
chapter: true
pre: "<b>5.</b>"
---

# EduCloud Lite – AWS Cloud Learning Platform

Welcome to the technical workshop for **EduCloud Lite**, a cloud-based learning
management platform built with React, FastAPI, Amazon Cognito, Amazon S3,
CloudFront, AWS Amplify, Elastic Beanstalk, and a managed PostgreSQL database.
This guide explains the decisions behind the architecture and provides a
repeatable path for deploying the application in your own AWS account.

- **Live application:** [Open EduCloud Lite](https://main.djk00b5qbck73.amplifyapp.com/courses)
- **Architecture diagram:** [Download the editable draw.io file](../files/educloud-aws-architecture.drawio)
- **Source code:** [EduCloud on GitHub](https://github.com/Funacius/EduCloud)
- **Build and deployment guide:** [Download the PDF guide](../files/EduCloud-Build-Deployment-Guide.pdf)

![EduCloud Lite AWS architecture diagram](/images/educloud-aws-architecture.png)

## Overview

EduCloud Lite separates the browser interface, application API, identity
management, object storage, and database responsibilities. The frontend is
built and hosted by Amplify. FastAPI runs on Elastic Beanstalk. Cognito issues
the user identity tokens, while Supabase PostgreSQL stores application data.
Private course assets are uploaded to S3 and delivered through CloudFront with
Origin Access Control, so the bucket does not need to be public.

The workshop is intentionally practical: every section maps to a real
deployment task, includes the relevant AWS settings, and highlights the
security or cost reason behind the choice. Use your own AWS account and your
own database credentials; never copy secrets from the example screenshots.

## Documentation outline

Use this outline to move directly to a major part of the workshop:

1. [Introduction](5.1-overview/) – architecture and request flow
   - [Architecture](5.1-overview/5.1.1-architecture/)
   - [Request flow](5.1-overview/5.1.2-request-flow/)
2. [Prerequisites](5.2-prerequisites/) – accounts, tools, and repository setup
3. [Secrets and database](5.3-secrets-and-database/) – Supabase, Parameter Store, and IAM
4. [Authentication with Cognito](5.4-authentication/) – user pool, app client, and first sign-in
5. [Deploy the backend](5.5-backend/) – FastAPI packaging and Elastic Beanstalk
6. [Private storage and CloudFront](5.6-storage-delivery/) – S3, OAC, and delivery paths
7. [Deploy the frontend](5.7-frontend/) – Amplify build variables and SPA routing
8. [Validation and troubleshooting](5.8-validation/) – test the live application
9. [Cleanup](5.9-cleanup/) – remove resources when the submission is complete
