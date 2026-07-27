---
title: "Workshop"
weight: 5
chapter: false
pre: "<b>5.</b>"
---

# Deploy EduCloud Lite on AWS

This hands-on workshop guides readers through deploying their own EduCloud Lite
environment. Every participant must use their own AWS account, Cognito User Pool,
S3 bucket, Parameter Store values, and PostgreSQL database.

## Workshop modules

1. [Introduction](5.1-overview/)
2. [Prerequisites](5.2-prerequisites/)
3. [Secrets and database](5.3-secrets-and-database/)
4. [Authentication with Cognito](5.4-authentication/)
5. [Deploy the backend](5.5-backend/)
6. [Private storage and CloudFront](5.6-storage-delivery/)
7. [Deploy the frontend](5.7-frontend/)
8. [Validation and troubleshooting](5.8-validation/)
9. [Cleanup](5.9-cleanup/)

## Suggested screenshots

The workshop will look closer to an AWS hands-on lab if each major step includes
one screenshot. Save the images under `static/images/workshop/` using the
suggested names below:

| Section | Screenshot content | File name |
|---|---|---|
| 5.1 | Overall AWS architecture | `/images/educloud-aws-architecture.png` |
| 5.4 | Cognito User Pool | `01-cognito-user-pool.png` |
| 5.3 | Supabase Session Pooler | `02-supabase-session-pooler.png` |
| 5.3 | Supabase Table Editor | `02b-supabase-table-editor.png` |
| 5.3 | Parameter Store SecureString values | `03-ssm-secure-parameters.png` |
| 5.3 | IAM policy for the EC2 role | `04-eb-ec2-role-ssm-policy.png` |
| 5.5 | Elastic Beanstalk health is Green | `05-elastic-beanstalk-green.png` |
| 5.6 | S3 bucket with Block Public Access | `06-s3-private-bucket.png` |
| 5.6 | CloudFront origins and behaviors | `07-cloudfront-origins-behaviors.png` |
| 5.7 | Amplify deployment succeeded | `08-amplify-deployed.png` |
| 5.8 | Live EduCloud Lite application | `09-live-application.png` |

## Estimated duration

Approximately 3–4 hours when the source code and AWS account are ready.

## Final result

- React frontend hosted by Amplify.
- FastAPI backend running on Elastic Beanstalk.
- Authentication managed by Cognito.
- Private course files stored in S3 and delivered by CloudFront.
- PostgreSQL connection and JWT secret loaded from Parameter Store.
