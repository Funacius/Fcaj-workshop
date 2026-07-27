---
title: "Auto Confirm Lambda"
weight: 3
chapter: false
pre: "<b>5.4.3.</b>"
---

# Optional Auto Confirm Lambda

The repository contains:

```text
aws/cognito-pre-signup/index.mjs
```

If you want self-service signups to be confirmed automatically:

1. Create a Node.js Lambda function in the same Region.
2. Paste and deploy the function code.
3. Attach it to the User Pool as the **Pre sign-up Lambda trigger**.

This trigger confirms and verifies self-service signups. It does not replace the
email delivery used by the Forgot Password flow.

If you prefer users to confirm registration themselves, do not attach this
trigger. Keep email recovery enabled in either case.

Application roles are still stored in PostgreSQL:

- New public users become Students.
- Instructor applicants remain Students until approved by an Admin.
- Admin accounts must be provisioned explicitly.

