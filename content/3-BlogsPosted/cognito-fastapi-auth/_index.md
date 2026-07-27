---
title: "Authentication Design with Amazon Cognito and FastAPI"
menuTitle: "Cognito and FastAPI Auth"
weight: 3
pre: "<b>3.3.</b>"
---

## Authentication Design with Amazon Cognito and FastAPI

Hello AWS Study Group VN! Authentication was one of the biggest changes when
EduCloud Lite moved from local testing to production. Local demo accounts were
useful during development, but a public website needs a safer identity flow for
passwords, account recovery, and first-time users.

This blog summarizes how EduCloud Lite uses Amazon Cognito for identity while
FastAPI and PostgreSQL keep control of application roles such as Student,
Instructor, and Admin.

## 1. Why Cognito Was Added

During early development, EduCloud Lite supported legacy local authentication.
This was convenient because it allowed quick testing with sample users such as
Student, Instructor, and Admin.

However, legacy authentication is not suitable for a public deployment because:

- Password handling should not be improvised inside application code.
- Forgot-password email flow is difficult to implement securely from scratch.
- Provisioned users need a clean first-login password setup.
- Public login should avoid exposing whether an email exists.
- Authentication should be separated from business role management.

Amazon Cognito User Pools solved the identity side of the problem and allowed
the project to keep business authorization inside the backend.

## 2. Authentication vs Authorization

One design lesson from EduCloud Lite is that authentication and authorization
should not be mixed.

| Question | System responsible | Example |
| --- | --- | --- |
| Who is this user? | Amazon Cognito | Email, password, user pool subject |
| Is this login token valid? | FastAPI backend | Verify Cognito JWT signature and claims |
| What can this user do? | EduCloud database | Student, Instructor, Admin role |
| What data can this user access? | Backend services | Own courses, enrolled courses, admin dashboard |

Cognito identifies the user. EduCloud decides the role and permission inside
the learning platform.

This separation helped solve an important issue: a user can be confirmed in
Cognito but still have the wrong EduCloud role if the database record is not
mapped correctly. The backend must treat the database role as the source of
truth.

## 3. Production Login Flow

The production login flow works like this:

1. The user enters email and password in the React frontend.
2. The frontend authenticates against Cognito.
3. Cognito returns identity tokens after a successful sign-in.
4. The frontend sends the Cognito ID token to the FastAPI backend.
5. The backend verifies the token signature and checks the user pool claims.
6. The backend maps Cognito `sub` to the EduCloud user record.
7. The backend returns an application session/JWT containing EduCloud role data.

This means the frontend does not decide whether someone is an Instructor or
Admin. It only displays what the backend returns after verification.

## 4. First-Login Password Setup

EduCloud Lite supports provisioned accounts. This is useful when an admin wants
to create accounts for instructors or demo users before the first login.

The flow is:

1. Admin creates a Cognito user with a temporary password.
2. The user signs in for the first time.
3. Cognito returns a `NEW_PASSWORD_REQUIRED` challenge.
4. The frontend shows a "Set your permanent password" page.
5. The user enters and confirms a new password.
6. The frontend completes the challenge with Cognito.
7. After successful login, EduCloud checks whether profile information is
   complete and routes the user accordingly.

This creates a smoother onboarding flow than manually sharing permanent
passwords.

## 5. Forgot Password Flow

For password recovery, Cognito handles the email verification code. EduCloud
does not need to store reset tokens in the application database.

The flow is:

1. User clicks "Forgot password?".
2. User enters email.
3. Cognito sends a reset code to the user email.
4. User enters the code and a new password.
5. Cognito updates the password.
6. User signs in again with the new password.

For this project, email is enough. SMS was not required, which keeps the setup
simple and avoids unnecessary cost or SNS configuration.

## 6. Email Confirmation Strategy

EduCloud Lite supports different account styles:

- Public/self-created users may need email confirmation.
- Admin-provisioned users can be confirmed depending on the selected setup.
- Forgot-password still uses email delivery even when normal account activation
  is handled by the project workflow.

This distinction is important. Email confirmation proves ownership of an email
address, while EduCloud role approval proves whether the user should become an
Instructor.

For example, an account can have a verified email but still remain a Student
until the Admin approves an Instructor request.

## 7. Backend Security Checks

FastAPI should not trust frontend claims directly. The backend authentication
layer is responsible for:

- Reading Cognito public keys.
- Verifying JWT signature.
- Checking issuer and audience/client values.
- Extracting the Cognito `sub`.
- Mapping the `sub` to a database user.
- Returning only the application role stored in EduCloud.

This avoids a common mistake: trusting role information sent from the browser.
The browser is a client, not an authority.

## 8. Problems Encountered

| Problem | Cause | Fix |
| --- | --- | --- |
| Login showed "legacy unauthenticated" | Frontend/backend were still using legacy mode or old environment values | Updated production environment variables and disabled legacy auth |
| User was confirmed but still could not sign in | Email verification and account state were inconsistent | Adjusted Cognito settings and confirmation behavior |
| Instructor account became Student | Cognito identity existed, but EduCloud database role was still Student | Updated the database role and kept role source server-side |
| First-login page showed missing profile data | Provisioned account lacked required profile fields | Added routing to profile completion before normal use |
| Amplify login failed with `Failed to fetch` | API base URL, CORS, or CloudFront behavior was incorrect | Updated `VITE_API_BASE_URL`, backend CORS, and CloudFront API behavior |

## 9. Key Learnings

- **Cognito is identity, not the full application:** It should not replace
  domain-specific role logic.
- **Database role should remain authoritative:** Student, Instructor, and Admin
  permissions belong to EduCloud business rules.
- **First-login flow improves account provisioning:** Temporary accounts become
  usable without exposing permanent credentials.
- **Forgot-password should be managed:** Using Cognito avoids building a custom
  reset-token system.
- **Environment variables decide production behavior:** A wrong User Pool ID,
  Client ID, or legacy flag can make a deployed frontend behave like local
  development.

## 10. Future Improvements

- Move to secure HTTP-only cookie sessions for stronger browser token handling.
- Add rate limiting and account lockout monitoring.
- Add clearer Admin tools for creating and promoting users.
- Add end-to-end tests for login, first-login password setup, and password
  recovery.
- Use infrastructure as code to reproduce Cognito settings consistently.

## Conclusion

Adding Amazon Cognito made EduCloud Lite closer to a real production web
application. Cognito manages identity, passwords, first login, and recovery,
while the FastAPI backend protects the application rules.

The most important lesson is separation of responsibility. Cognito should answer
"who is signing in?", but the EduCloud backend must answer "what is this user
allowed to do?"

**Source:** EduCloud Lite project repository and deployment report.  
**Repository:** [https://github.com/Funacius/EduCloud](https://github.com/Funacius/EduCloud)  
**Live application:** [https://main.djk00b5qbck73.amplifyapp.com/](https://main.djk00b5qbck73.amplifyapp.com/)
