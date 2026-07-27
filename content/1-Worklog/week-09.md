---
title: "Week 9 - Cognito and security hardening"
menuTitle: "Week 9"
weight: 9
pre: "<b>1.9.</b>"
---

**Period:** August 3, 2026 - August 7, 2026

## Objectives

- Move production identity management to Amazon Cognito.
- Complete confirmation, first-login, and password-recovery flows.
- Separate identity from application role and harden APIs.

## AWS Learning Phase

| Learning content | Reference | Application in EduCloud |
| --- | --- | --- |
| Cognito User Pools, app clients, user confirmation, password reset, and JWT validation. | [Amazon Cognito Workshop](https://000141.awsstudygroup.com/) | Migrated production identity management from local auth to Cognito. |
| Amplify authentication integration patterns with Cognito. | [Amplify Authentication and Storage](https://000134.awsstudygroup.com/) | Connected the React sign-in, first-login, and recovery screens to Cognito. |

## Work completed

| Date | Activity | Outcome |
| --- | --- | --- |
| Aug 03 | Created the Singapore Cognito User Pool/App Client with password and email recovery settings. | Delegated passwords and recovery codes to Cognito. |
| Aug 04 | Integrated signup, confirmation, resend, and login using Cognito Identity JS. | Completed account lifecycle UI. |
| Aug 05 | Verified/exchanged Cognito ID tokens and mapped `sub` to the Supabase user. | Preserved database-authoritative roles. |
| Aug 06 | Implemented NEW_PASSWORD_REQUIRED and profile-first routing for provisioned users. | Supported safe temporary-account onboarding. |
| Aug 07 | Added forgot/reset, generic anti-enumeration responses, optional pre-sign-up Lambda, and production auth flags. | Hardened the production authentication path. |

## Achievements

- Cognito owns identity while Supabase owns profile and role.
- Added email confirmation, reset, resend, and first-login password flows.
- Validated Cognito signatures before issuing an EduCloud JWT.
- Added throttling, security headers, and production legacy-auth controls.

## Project evidence

- [Backend Cognito service](https://github.com/Funacius/EduCloud/blob/main/backend/app/services/cognito_service.py)
- [Frontend Cognito service](https://github.com/Funacius/EduCloud/blob/main/frontend/src/services/cognitoService.ts)
- [Pre-sign-up Lambda](https://github.com/Funacius/EduCloud/tree/main/aws/cognito-pre-signup)
- [Cognito tests](https://github.com/Funacius/EduCloud/blob/main/backend/tests/test_cognito_exchange.py)
