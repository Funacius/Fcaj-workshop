---
title: "Week 4 - Basic authentication and authorization"
menuTitle: "Week 4"
weight: 4
pre: "<b>1.4.</b>"
---

**Period:** June 29, 2026 - July 3, 2026

## Objectives

- Implement development registration, login, and JWT sessions.
- Protect APIs and pages by user role.
- Keep authentication replaceable for later Cognito integration.

## AWS Learning Phase

| Learning content | Reference | Application in EduCloud |
| --- | --- | --- |
| IAM policy structure and least-privilege authorization. | [Access Control with AWS IAM](https://000002.awsstudygroup.com/) | Built a similar permission mindset for Admin, Instructor, and Student routes. |
| User Pools, app clients, JWTs, and the difference between authentication and authorization. | [Amazon Cognito Workshop](https://000141.awsstudygroup.com/) | Prepared the authentication layer so it could later migrate from local JWT to Cognito. |

## Work completed

| Date | Activity | Outcome |
| --- | --- | --- |
| Jun 29 | Added user schemas, password hashing, and development register/login APIs. | Enabled local account authentication. |
| Jun 30 | Issued JWTs containing identity, role, and expiry; parsed Bearer tokens. | Identified users on protected requests. |
| Jul 01 | Added auth middleware and Student/Instructor/Admin authorization helpers. | Restricted mutations to valid roles. |
| Jul 02 | Added AuthContext, session persistence, token attachment, and RequireRole. | Protected frontend navigation and pages. |
| Jul 03 | Tested cross-role access and added 401/403 responses, security headers, and auth throttling. | Reduced privilege-escalation risks. |

## Achievements

- Completed development authentication and 12-hour JWT sessions.
- Protected Instructor and Admin functionality on both application layers.
- Prevented public registration from assigning Admin.
- Preserved a clean path for Cognito migration.

## Project evidence

- [Authentication routes](https://github.com/Funacius/EduCloud/blob/main/backend/app/routes/auth_routes.py)
- [Auth middleware](https://github.com/Funacius/EduCloud/blob/main/backend/app/middleware/auth_middleware.py)
- [Frontend AuthContext](https://github.com/Funacius/EduCloud/blob/main/frontend/src/auth/AuthContext.tsx)
