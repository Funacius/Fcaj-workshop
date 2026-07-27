---
title: "Configure App Client"
weight: 2
chapter: false
pre: "<b>5.4.2.</b>"
---

# Configure the App Client

Create a public app client for the React frontend:

- App type: public client.
- Client secret: disabled.
- Authentication flows: username/password or the default recommended options.
- Callback/sign-out URLs: add the Amplify domain if hosted UI is used.

Record:

```text
COGNITO_CLIENT_ID
```

The frontend uses `VITE_COGNITO_REGION`, `VITE_COGNITO_USER_POOL_ID`, and
`VITE_COGNITO_CLIENT_ID`. These values are public identifiers; do not put a
Cognito client secret into a browser application.

