---
title: "Prerequisites"
weight: 2
chapter: false
pre: "<b>5.2.</b>"
---

# Prerequisites

## Accounts

- An AWS account with permission to use IAM, Cognito, Lambda, Systems Manager,
  Elastic Beanstalk, S3, CloudFront, CloudWatch, and Amplify.
- A GitHub account and a repository containing EduCloud.
- A Supabase account, or another reachable PostgreSQL database.

## Local tools

- Git
- Python 3.11 or newer
- Node.js 18 or newer
- AWS CLI v2
- PowerShell

Verify the tools:

```powershell
git --version
python --version
node --version
npm --version
aws --version
```

Clone the source:

```powershell
git clone https://github.com/Funacius/EduCloud.git
Set-Location EduCloud
```

## Region

Use one AWS Region consistently. This workshop uses Singapore:

```text
ap-southeast-1
```

Create Parameter Store values, Cognito, Lambda, Elastic Beanstalk, and S3 in this
Region. CloudFront and Amplify are global services.

> Note: Enable an AWS Budget before creating resources. Do not use the Elastic
> Beanstalk service bucket for application uploads; create a separate private
> bucket.
