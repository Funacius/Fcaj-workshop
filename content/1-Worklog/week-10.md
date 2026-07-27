---
title: "Week 10 - Secrets and backend deployment"
menuTitle: "Week 10"
weight: 10
pre: "<b>1.10.</b>"
---

**Period:** August 10, 2026 - August 14, 2026

## Objectives

- Remove production secrets from source code.
- Deploy FastAPI to AWS Elastic Beanstalk.
- Validate health, logging, and the remote Supabase connection.

## AWS Learning Phase

| Learning content | Reference | Application in EduCloud |
| --- | --- | --- |
| Managing database credentials and secrets outside source code. | [Secrets Manager with RDS](https://000096.awsstudygroup.com/) | Applied the same principle with SSM Parameter Store for `DATABASE_URL` and `JWT_SECRET_KEY`. |
| Instance roles, development/production environments, health checks, and application updates on Elastic Beanstalk. | [Deploy Application Using Elastic Beanstalk](https://000112.awsstudygroup.com/) | Deployed FastAPI on Python 3.12 and Amazon Linux 2023. |

## Work completed

| Date | Activity | Outcome |
| --- | --- | --- |
| Aug 10 | Created encrypted Parameter Store entries for `DATABASE_URL` and `JWT_SECRET_KEY` in `ap-southeast-1`. | Centralized secrets outside Git. |
| Aug 11 | Created the Beanstalk service role, EC2 instance profile, and least-privilege SSM policy. | Limited runtime access to required parameters. |
| Aug 12 | Prepared Procfile, requirements, port 8000, entry point, and backend source bundle. | Matched the Python 3.12 Beanstalk platform. |
| Aug 13 | Created a Single Instance environment and configured Cognito, CORS, flags, and SSM references. | Reached Green environment health. |
| Aug 14 | Checked root/docs/health, SSL database access, and web logs; fixed startup dependencies. | Stabilized the production backend. |

## Achievements

- Kept database and JWT secrets out of GitHub and Amplify.
- Used the EC2 role to read only required parameters.
- Ran FastAPI behind Nginx/uvicorn with Green health.
- Established repeatable log inspection and redeployment.

## Project evidence

- [Backend Procfile](https://github.com/Funacius/EduCloud/blob/main/backend/Procfile)
- [Production config](https://github.com/Funacius/EduCloud/blob/main/backend/app/config.py)
- [Backend documentation](https://github.com/Funacius/EduCloud/blob/main/backend/README.md)
