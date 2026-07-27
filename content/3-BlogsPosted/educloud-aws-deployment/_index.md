---
title: "Deploying EduCloud Lite on AWS"
menuTitle: "EduCloud AWS Deployment"
weight: 1
pre: "<b>3.1.</b>"
---

## Deploying EduCloud Lite on AWS: From Local LMS to Public Website

Hello AWS Study Group VN! During my First Cloud AI Journey internship, I built
**EduCloud Lite**, a lightweight learning management platform that supports
course publishing, student enrollment, lesson progress tracking, final
assessments, and completion certificates.

This article summarizes how I moved the project from a local React/FastAPI
application to an AWS-based deployment with managed identity, private storage,
HTTPS delivery, and a public frontend link.

## 1. Project Context and Goal

EduCloud Lite started as a full-stack web application with three main roles:

- **Student:** Browse courses, enroll, learn lessons, complete assessments, and
  view certificates.
- **Instructor:** Create courses, upload thumbnails/resources, manage lessons,
  configure final assessments, and publish content.
- **Admin:** Review Instructor requests, manage users/courses, and monitor basic
  system health.

The deployment goal was not to build a large enterprise LMS. The goal was to
create a working, understandable, and cost-conscious cloud architecture that can
be demonstrated through one independent website link.

## 2. Technical Highlights

### Separating identity, application data, and storage

The first design decision was to avoid mixing all responsibilities into one
server.

| Responsibility | Service / Component | Why it was used |
| --- | --- | --- |
| Browser application | React, TypeScript, Vite, Amplify Hosting | Fast frontend build and public static hosting |
| Business API | FastAPI on Elastic Beanstalk | Python backend with clear REST endpoints |
| User identity | Amazon Cognito | Managed passwords, confirmation, first login, and recovery |
| Application data | Supabase PostgreSQL | Relational data for users, courses, lessons, progress, attempts, and certificates |
| Private course assets | Amazon S3 | Object storage for thumbnails, videos, and materials |
| HTTPS delivery and routing | Amazon CloudFront | One delivery layer for API and private course assets |
| Secrets | Systems Manager Parameter Store | Keep database URL and JWT secret outside source code |

This separation made the application easier to troubleshoot. When login failed,
I checked Cognito and token exchange. When file loading failed, I checked S3,
CloudFront behavior, bucket policy, and CORS. When API calls failed, I checked
Elastic Beanstalk health and backend logs.

### Production request flow

The final request flow is:

1. A user opens the React frontend from Amplify Hosting.
2. The browser signs in through Amazon Cognito.
3. The frontend sends API requests to CloudFront under `/api/*`.
4. CloudFront forwards API traffic to Elastic Beanstalk.
5. FastAPI validates tokens, applies role rules, and reads/writes Supabase
   PostgreSQL.
6. Uploaded course files are stored in S3.
7. Course assets are delivered through CloudFront under `/courses/*` using
   private S3 access.

## 3. Practical Deployment Steps

### Backend deployment with Elastic Beanstalk

FastAPI was deployed on Elastic Beanstalk using the Python 3.12 platform. The
backend bundle included the application source, dependencies, and a `Procfile`
that starts the API server.

Important configuration values were not pasted directly into source code.
Instead, the deployment used Parameter Store references for:

- `DATABASE_URL`
- `JWT_SECRET_KEY`

The Elastic Beanstalk EC2 instance profile was granted read access only to the
specific parameters required by EduCloud Lite.

### Cognito authentication

Cognito was used as the identity provider. It handled:

- User sign-in with email.
- Forgot password and reset code flow.
- First-login password challenge for provisioned users.
- Email verification depending on the chosen account setup.

The application still stores the role in PostgreSQL. Cognito answers the
question "Who is this identity?", while EduCloud answers "What can this user do
inside the LMS?"

### Private storage with S3 and CloudFront

Course assets were stored in a dedicated S3 bucket, not in the Elastic Beanstalk
service bucket. The upload bucket uses:

- Block Public Access enabled.
- ACLs disabled.
- Server-side encryption with SSE-S3.
- Bucket policy scoped to CloudFront Origin Access Control.

CloudFront has separate behaviors:

| Path pattern | Origin | Cache policy | Purpose |
| --- | --- | --- | --- |
| `Default (*)` / `/api/*` | Elastic Beanstalk | Caching disabled | Dynamic API requests |
| `/courses/*` | Private S3 origin | Caching optimized | Course thumbnails, videos, and materials |

This allowed course assets to be served efficiently without making the S3 bucket
public.

### Frontend deployment with Amplify Hosting

The frontend was deployed from GitHub using Amplify Hosting. The monorepo app
root was set to `frontend`, with:

- Build command: `npm run build`
- Output directory: `dist`
- SPA rewrite to `/index.html`

The frontend build used `VITE_*` environment variables for values that are safe
to expose in browser code, such as:

- `VITE_API_BASE_URL`
- `VITE_COGNITO_REGION`
- `VITE_COGNITO_USER_POOL_ID`
- `VITE_COGNITO_CLIENT_ID`

Database URLs, JWT secrets, and AWS credentials were never added to the frontend
environment.

## 4. Problems Encountered

| Problem | Root Cause | Resolution |
| --- | --- | --- |
| Backend failed during startup | Missing dependency and incorrect production environment assumptions | Checked Elastic Beanstalk logs and fixed backend requirements/config |
| Cognito user could sign in but had wrong application role | Identity existed in Cognito but role mapping was controlled by Supabase | Updated the user record in PostgreSQL and kept role assignment server-side |
| `Failed to fetch` from Amplify frontend | CORS and API origin mismatch | Updated backend `CORS_ORIGINS` and frontend `VITE_API_BASE_URL` |
| S3 asset returned 403 | CloudFront origin access and bucket policy were incomplete | Added OAC and the generated bucket policy while keeping Block Public Access enabled |
| Refreshing `/login` or `/profile` failed | React uses client-side routing | Added Amplify SPA rewrite to `/index.html` |

## 5. AWS Services in the Architecture

- **AWS Amplify Hosting:** Builds and hosts the React frontend from GitHub.
- **Amazon CloudFront:** Routes API traffic and delivers private course assets.
- **AWS Elastic Beanstalk:** Runs the FastAPI backend.
- **Amazon Cognito:** Manages user identity, sign-in, confirmation, and password
  recovery.
- **Amazon S3:** Stores uploaded course thumbnails, videos, and materials.
- **AWS Systems Manager Parameter Store:** Stores production secrets.
- **AWS IAM:** Grants least-privilege runtime permissions.
- **Amazon CloudWatch:** Supports logs and health troubleshooting.

## 6. Key Learnings

- **Separate identity from application role:** Cognito should manage login, but
  the application database should decide Student, Instructor, and Admin access.
- **Do not publicize S3 just to make files load:** CloudFront OAC allows private
  delivery without disabling Block Public Access.
- **Frontend variables are public:** Only `VITE_*` values safe for browsers
  should be used in Amplify.
- **Production debugging is layered:** API errors, CORS, Cognito, CloudFront,
  S3, and database connectivity must be checked one layer at a time.
- **Cost control matters:** A single-instance backend, private S3, and careful
  logging choices are enough for an internship submission.

## 7. Limitations and Future Improvements

- Add infrastructure as code to reduce manual AWS Console steps.
- Add end-to-end browser tests for Student, Instructor, and Admin flows.
- Move database migration management to Alembic for production-grade releases.
- Add CloudWatch alarms and a shared monitoring dashboard.
- Harden token storage and cookie/session strategy for a larger production
  environment.

## Conclusion

Deploying EduCloud Lite on AWS showed that a student project can still follow
real cloud architecture principles: managed identity, private storage,
least-privilege IAM, externalized secrets, HTTPS delivery, and reproducible
documentation.

The most important lesson is that "it works locally" is only the first stage.
The real engineering work begins when authentication, networking, storage
permissions, CORS, logging, and cost control all need to work together.

**Source:** EduCloud Lite project repository and deployment report.  
**Repository:** [https://github.com/Funacius/EduCloud](https://github.com/Funacius/EduCloud)  
**Live application:** [https://main.djk00b5qbck73.amplifyapp.com/](https://main.djk00b5qbck73.amplifyapp.com/)
