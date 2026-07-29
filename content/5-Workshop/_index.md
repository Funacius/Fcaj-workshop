---
title: "Workshop"
weight: 5
chapter: true
pre: "<b>5.</b>"
---

# EduCloud Lite – Cloud Learning Platform on AWS

Welcome to the technical workshop for **EduCloud Lite**, a cloud-based learning management platform for students, instructors, and administrators. The project is built with React, FastAPI, Amazon Cognito, Amazon S3, CloudFront, AWS Amplify, Elastic Beanstalk, AWS Systems Manager Parameter Store, and Supabase PostgreSQL.

- **Live application:** [Open EduCloud Lite](https://main.djk00b5qbck73.amplifyapp.com/courses)
- **Source code:** [EduCloud on GitHub](https://github.com/Funacius/EduCloud)
- **Architecture diagram:** [Download the editable draw.io file](../files/educloud-aws-architecture.drawio)
- **Build and deployment guide:** [Download the PDF guide](../files/EduCloud-Build-Deployment-Guide.pdf)

## Project context and problem statement

Online learning platforms need more than a page that displays lessons. Instructors need a practical way to create courses, organize lessons, upload learning resources, create final assessments, and issue certificates. Students need a clear learning flow where they can discover published courses, enroll, study, track progress, complete assessments, and receive a result they can verify.

The challenge is to provide those features as one dependable web application while keeping identity, business data, uploaded media, and production secrets separate. A local prototype is not sufficient: the platform must be publicly accessible, secure enough for real accounts, and simple enough for an instructor or reviewer to test.

EduCloud Lite addresses this problem with a role-based learning platform. The system keeps the learning workflow focused and avoids unnecessary e-commerce complexity: all courses in the current scope are free, so checkout and payment processing are intentionally excluded.

## Project objectives

The project was designed to deliver a complete, demonstrable cloud application rather than isolated AWS service examples.

- Allow **students** to browse courses, enroll, learn from lessons, track progress, take a final assessment, and view a completion certificate.
- Allow **instructors** to create and publish courses, manage lessons and learning outcomes, upload thumbnails and course assets, and define assessments with multiple options and correct-answer rules.
- Allow **administrators** to manage roles and instructor requests, review application health, inspect recent logs, and view storage and database statistics.
- Use AWS services in a way that is appropriate for a student project: secure by default, easy to demonstrate, and mindful of operating cost.

## Target users

### Students

Students are the primary learners. They need a low-friction path from registration to enrollment and course completion. The interface presents course information, lessons, assessment progress, and certificates in one place so that learners can see what to do next.

### Instructors

Instructors need authoring tools without having to manage infrastructure. They can create course content, choose a thumbnail, add lessons, create final-assignment questions, and publish the course once the required content is ready.

### Administrators

Administrators oversee the platform. Their dashboard focuses on operational tasks: user roles, instructor applications, recent Elastic Beanstalk logs, health signals, S3 usage, database statistics, and available AWS cost information.

## Core technical challenges

### 1. Role-based identity and account recovery

The platform must distinguish student, instructor, and administrator permissions. Amazon Cognito manages registration, email verification, sign-in, first-login password changes, and password recovery. The backend validates Cognito tokens and maps the authenticated identity to the application role stored in Supabase.

### 2. Consistent learning and assessment data

Course progress, enrollments, assessments, certificates, reviews, and instructor applications are related records. The API keeps the business rules on the server so that a learner cannot obtain a certificate merely by changing browser-side state.

### 3. Private media delivery

Course thumbnails, videos, and documents should not live on the Elastic Beanstalk instance filesystem or be exposed through a public S3 bucket. The backend uses presigned URLs for direct uploads, S3 stores objects privately, and CloudFront serves approved course asset paths through Origin Access Control (OAC).

### 4. Deploying a browser application and API together

The React application and FastAPI API are deployed independently. Amplify builds the frontend from the GitHub `main` branch; Elastic Beanstalk runs the Python application. CloudFront routes `/api/*` traffic to Elastic Beanstalk and `/courses/*` assets to S3, while Amplify provides the public web interface.

### 5. Security, monitoring, and cost awareness

Production secrets are stored in Systems Manager Parameter Store rather than in the repository. IAM roles follow least privilege. Elastic Beanstalk sends application logs to CloudWatch, and the Admin area exposes useful operational information without requiring a reviewer to search multiple AWS consoles.

## Solution architecture

![EduCloud Lite AWS architecture diagram](/images/educloud-aws-architecture.png)

- **Frontend:** React, TypeScript, and Vite are hosted on AWS Amplify and automatically deployed from GitHub.
- **Backend:** FastAPI runs on Amazon EC2 through Elastic Beanstalk and provides APIs for courses, lessons, enrollment, progress, assessments, reviews, certificates, uploads, and administration.
- **Authentication:** Cognito handles account lifecycle; Supabase PostgreSQL stores the application profile and role mapping.
- **Storage and delivery:** S3 privately stores course assets; CloudFront delivers them and routes API traffic to the backend.
- **Operations:** Parameter Store, IAM, CloudWatch, and Elastic Beanstalk health reporting support secure configuration and troubleshooting.

## Workshop outline

Use this guide to reproduce the EduCloud Lite deployment in your own account. Each section documents the real configuration used for the project and explains the reason for the decision.

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
