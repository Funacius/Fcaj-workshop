---
title: "Introduction"
weight: 1
chapter: false
disableTitle: true
pre: "<b>5.1.</b>"
---

# INTRODUCTION

This introduction explains the EduCloud Lite deployment architecture before any AWS resources are created. It provides the project context, the problems the platform solves, and the technical decisions that connect the later hands-on steps.

## Project context and problem statement

An online learning platform needs more than a page that displays lessons. Instructors need a practical way to create courses, organize lessons, upload learning resources, create final assessments, and issue certificates. Students need a clear path to discover published courses, enroll, study, track progress, complete an assessment, and receive a result they can verify.

The challenge is to provide those capabilities as one dependable web application while keeping identity, business data, uploaded media, and production secrets separate. A local prototype is not sufficient: the platform must be publicly accessible, secure enough for real accounts, and simple enough for an instructor or reviewer to test.

EduCloud Lite addresses this problem with a role-based learning platform. The current scope intentionally keeps every course free, so checkout and payment processing are not included.

## Project objectives

The project aims to deliver a complete, demonstrable cloud application rather than isolated AWS service examples.

- Allow **students** to browse courses, enroll, learn from lessons, track progress, take a final assessment, and view a completion certificate.
- Allow **instructors** to create and publish courses, manage lessons and learning outcomes, upload thumbnails and course assets, and define assessment questions with multiple options and correct-answer rules.
- Allow **administrators** to manage roles and instructor requests, review application health, inspect recent logs, and view storage and database statistics.
- Use AWS services in a way that is secure by default, easy to demonstrate, and mindful of operating cost.

## Target users

### Students

Students are the primary learners. They need a low-friction path from registration to enrollment and course completion. The interface presents course information, lessons, assessment progress, and certificates in one place so learners always know what to do next.

### Instructors

Instructors need authoring tools without having to manage infrastructure. They can create course content, choose a thumbnail, add lessons, create final-assignment questions, and publish the course once the required content is ready.

### Administrators

Administrators oversee the platform. Their dashboard focuses on user roles, instructor applications, recent Elastic Beanstalk logs, health signals, S3 usage, database statistics, and available AWS cost information.

## Core technical challenges

### 1. Role-based identity and account recovery

Amazon Cognito manages registration, email verification, sign-in, first-login password changes, and password recovery. The backend validates Cognito tokens and maps the authenticated identity to the application role stored in Supabase.

### 2. Consistent learning and assessment data

Course progress, enrollments, assessments, certificates, reviews, and instructor applications are related records. The API keeps business rules on the server so a learner cannot obtain a certificate merely by changing browser-side state.

### 3. Private media delivery

Course thumbnails, videos, and documents should not live on the Elastic Beanstalk instance filesystem or be exposed through a public S3 bucket. The backend uses presigned URLs for direct uploads, S3 stores objects privately, and CloudFront serves approved course asset paths through Origin Access Control (OAC).

### 4. Independent frontend and API deployment

Amplify builds and hosts the React frontend from the GitHub `main` branch. Elastic Beanstalk runs the FastAPI application. CloudFront routes `/api/*` traffic to Elastic Beanstalk and `/courses/*` assets to S3, while Amplify provides the public web interface.

### 5. Security, monitoring, and cost awareness

Production secrets are stored in Systems Manager Parameter Store rather than in the repository. IAM roles follow least privilege. Elastic Beanstalk streams logs to CloudWatch, and the Admin area displays useful operational information for troubleshooting.

## Documentation outline

1. [Architecture](5.1.1-architecture/) – components and responsibility boundaries
2. [Request flow](5.1.2-request-flow/) – browser, authentication, API, database, and media paths
