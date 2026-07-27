---
title: "Week 12 - Testing, optimization, and report"
menuTitle: "Week 12"
weight: 12
pre: "<b>1.12.</b>"
---

**Reporting period:** August 24, 2026 - September 15, 2026

## Objectives

- Validate all three roles end to end in production.
- Fix UI, authentication, CORS, upload, and deployment issues.
- Complete Draw.io architecture, Hugo workshop, and submission artifacts.

## AWS Learning and Operations Review

| Learning content | Reference | Application in EduCloud |
| --- | --- | --- |
| Metrics, logs, alarms, and dashboards for application monitoring. | [AWS CloudWatch Workshop](https://000008.awsstudygroup.com/) | Reviewed Elastic Beanstalk health, logs, and operational indicators. |
| Budget alerts, cost tracking, and workshop cleanup. | [AWS Budgets](https://000007.awsstudygroup.com/) | Controlled credit usage, planned cleanup, and documented cost-conscious deployment choices. |
| Reviewing the full AWS learning path used by the project. | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/) | Matched the EduCloud architecture to the workshops used during implementation. |

## Work completed

| Milestone | Activity | Outcome |
| --- | --- | --- |
| Aug 24 | Ran isolated backend tests for Cognito, courses, enrollment, progress, Instructor review, and monitoring. | Verified behavior without modifying Supabase production. |
| Aug 28 | Ran the TypeScript production build and checked responsive UI, authoring, crop, and deletion rules. | Produced a stable frontend build. |
| Sep 03 | Tested Student profile, enrollment, learning, assessment, certificate, and resume end to end. | Validated course-completion conditions. |
| Sep 07 | Tested Instructor/Admin and production; corrected CORS, SPA rewrite, CloudFront, env, and auth state. | Enabled reliable access through an independent website link. |
| Sep 15 | Finalized Draw.io, evidence, bilingual Hugo workshop, worklog, self-assessment, cleanup, and links. | Prepared the repository, live site, and report for submission. |

## Achievements

- Added backend tests for critical business and security rules.
- Validated Student, Instructor, and Admin user journeys.
- Completed the AWS architecture and request-flow diagram.
- Produced a bilingual First Cloud Journey-style Hugo report.
- Documented remaining Alembic, server-side PDF, shared monitoring, and
  token/cookie hardening work.

## Project evidence

- [Backend tests](https://github.com/Funacius/EduCloud/tree/main/backend/tests)
- [API test plan](https://github.com/Funacius/EduCloud/blob/main/api/test-plan/test-cases.md)
- [Architecture source](https://github.com/Funacius/EduCloud/blob/main/docs/architecture/educloud-aws-architecture.drawio)
- [Project documentation](https://github.com/Funacius/EduCloud/blob/main/README.md)
