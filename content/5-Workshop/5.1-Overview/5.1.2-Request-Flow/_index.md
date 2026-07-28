---
title: "Request Flow"
weight: 2
chapter: false
pre: "<b>5.1.2.</b>"
---

# Request Flow

## Main flows

1. GitHub pushes to `main` trigger an Amplify build.
2. Users load the React/Vite SPA from Amplify Hosting.
3. The browser authenticates directly with the Cognito User Pool.
4. The browser sends API and media requests to CloudFront.
5. CloudFront forwards API requests to Elastic Beanstalk.
6. CloudFront forwards `/courses/*` requests to the private S3 origin using OAC.
7. FastAPI reads and writes application data in Supabase PostgreSQL over TLS.
8. FastAPI authorizes private object operations and returns short-lived multipart
   presigned URLs; the browser uploads video parts directly to S3.
9. S3 objects are delivered through CloudFront, while Elastic Beanstalk streams
   application logs to CloudWatch.
10. The EC2 instance profile permits the backend to read encrypted parameters,
    access the upload bucket, Cost Explorer, and the configured CloudWatch log group.

## Design intent

The request flow keeps frontend hosting, authentication, backend execution,
private media delivery, and database access separated. This makes the system
easier to troubleshoot because each failure can be mapped to a specific layer:
Amplify, Cognito, CloudFront, Elastic Beanstalk, S3, Parameter Store, or
PostgreSQL.
