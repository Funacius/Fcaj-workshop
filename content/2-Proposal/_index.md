---
title: "Proposal"
weight: 2
chapter: false
pre: "<b>2.</b>"
---

<div class="proposal-hero-title">EDUCLOUD LITE - CLOUD LEARNING PLATFORM ON AWS</div>

## 1. Project Overview

EduCloud Lite is a cloud-based learning management platform that allows
instructors to create courses, upload learning resources, manage final
assessments, and issue completion certificates. Students can browse published
courses, enroll, learn through lesson content, track progress, complete a final
assessment, and receive a certificate after meeting all requirements.

The project focuses on building a complete web application and deploying it with
AWS services in a way that is practical for an internship submission: clear
architecture, managed authentication, private storage, HTTPS delivery, and
cost-aware operations.

## 2. Problem Statement

Small training teams and workshop organizers often need more than static
documents or video links. They need a system that can manage users, publish
structured learning content, track student progress, evaluate final results, and
provide evidence of completion.

- **Limited learning control:** Static course pages do not verify enrollment,
  lesson completion, assessment attempts, or certificate eligibility.
- **Weak role separation:** A simple frontend-only website cannot safely protect
  Instructor and Admin actions.
- **Fragmented resources:** Course videos, materials, thumbnails, and
  certificates are often stored separately without a consistent delivery flow.
- **Deployment complexity:** A full LMS can become too complex or expensive for
  a student project if the architecture is not scoped carefully.

## 3. Project Objectives

The goal is to build an LMS that is usable, secure, and demonstrable through an
independent website link.

- **Course management:** Allow Instructors to create courses, lessons,
  thumbnails, resources, and publish-ready final assessments.
- **Student learning flow:** Support enrollment, lesson navigation, progress
  tracking, assessment submission, and certificate viewing.
- **Authentication and roles:** Use Amazon Cognito for identity and keep
  Student, Instructor, and Admin authorization inside the application database.
- **Private resource delivery:** Store course files in a private S3 bucket and
  deliver them through CloudFront without exposing the bucket publicly.
- **Reproducible deployment:** Document the AWS setup so another student can
  deploy the project with their own database, secrets, and AWS account.

## 4. Solution Architecture

EduCloud Lite uses a separated frontend, backend, identity, storage, and
database architecture.

![EduCloud AWS architecture](/images/educloud-aws-architecture.png)

- **Frontend:** React, TypeScript, and Vite provide the browser interface.
  Amplify Hosting builds and deploys the frontend from GitHub.
- **Backend:** FastAPI runs on Elastic Beanstalk and exposes REST APIs for
  courses, lessons, enrollment, progress, assessments, certificates, profiles,
  Instructor requests, and Admin operations.
- **Authentication:** Amazon Cognito manages user passwords, confirmation,
  first-login password changes, and password recovery. The backend validates
  Cognito tokens and maps identities to Supabase users.
- **Database:** Supabase PostgreSQL stores application data such as users,
  roles, courses, lessons, enrollments, progress, attempts, certificates, and
  review requests.
- **Storage and delivery:** Amazon S3 stores uploaded course assets privately.
  CloudFront routes `/api/*` traffic to Elastic Beanstalk and `/courses/*`
  assets to S3 through Origin Access Control.
- **Secrets and operations:** AWS Systems Manager Parameter Store stores
  production secrets. IAM roles grant the backend only the permissions it needs.
  CloudWatch and Elastic Beanstalk health are used for operational checks.

## 5. Deployment Timeline

- **Phase 1 - Preparation and AWS fundamentals:** Join the program, review Cloud
  Journey materials, set up tools, join the discussion group, and define the
  EduCloud Lite topic.
- **Phase 2 - Application foundation:** Design the React -> FastAPI ->
  PostgreSQL architecture and implement the core database models, routes, and
  development authentication.
- **Phase 3 - LMS features:** Build course authoring, lesson management,
  enrollment, progress tracking, final assessments, certificates, profiles, and
  Admin review flows.
- **Phase 4 - AWS integration:** Configure Cognito, Parameter Store, Elastic
  Beanstalk, S3, CloudFront, and Amplify Hosting.
- **Phase 5 - Validation and report:** Test Student, Instructor, and Admin
  flows, fix production issues, complete the Draw.io architecture diagram, and
  publish the Hugo workshop report.

## 6. Budget and Cost Optimization

The architecture is designed around a minimal submission cost while still using
real AWS services.

- **Single-instance backend:** Elastic Beanstalk is configured as a single
  instance environment to reduce fixed compute cost for demonstration traffic.
- **Private S3 with CloudFront:** Course assets are stored once in S3 and served
  through CloudFront instead of routing every asset request through the backend.
- **Managed identity:** Cognito removes the need to run a custom password and
  recovery service.
- **External managed PostgreSQL:** Supabase is used as the PostgreSQL database
  for this submission to avoid provisioning an additional RDS instance.
- **Cost guardrails:** AWS Budgets, limited logging, careful WAF choices, and a
  cleanup checklist help protect remaining credits.

## 7. Risk Assessment and Mitigation

| Risk Type | Problem Description | Mitigation Strategy |
| --- | --- | --- |
| Authentication mismatch | Cognito identities and database users can become inconsistent. | Map Cognito `sub` to the Supabase user record and keep application roles in PostgreSQL. |
| Secret leakage | Database URLs and JWT secrets can be exposed through code, screenshots, or frontend variables. | Store production secrets in Parameter Store and never place them in GitHub, Amplify `VITE_*` variables, or report screenshots. |
| Public media exposure | Course files could become public if S3 permissions are misconfigured. | Keep Block Public Access enabled, disable ACLs, use OAC, and scope bucket policies to CloudFront. |
| Cross-region latency | Supabase is outside `ap-southeast-1`, which may add latency. | Use the Supabase pooler with TLS and keep traffic volume low for the internship submission. |
| Broken SPA refresh | Direct browser refresh on `/login`, `/profile`, or `/instructor` can return 404. | Configure Amplify rewrite rules to serve `/index.html` for client-side routes. |
| AWS cost growth | Cloud services can continue charging after testing. | Use single-instance settings, budgets, and a documented cleanup order. |

## 8. Expected Outcomes

- A working public website that can be accessed through an independent Amplify
  link.
- A production backend running on Elastic Beanstalk and reachable through
  CloudFront.
- Secure Cognito authentication with Student, Instructor, and Admin application
  roles.
- Private course thumbnails, videos, and materials delivered through S3 and
  CloudFront.
- A complete workshop report showing architecture, deployment steps,
  troubleshooting, worklog, and evidence.
