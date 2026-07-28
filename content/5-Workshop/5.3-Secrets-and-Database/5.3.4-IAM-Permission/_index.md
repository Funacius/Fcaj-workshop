---
title: "IAM Permission"
weight: 4
chapter: false
pre: "<b>5.3.4.</b>"
---

# Grant IAM Permission

Attach an inline policy to the Elastic Beanstalk EC2 instance role. Replace the
account ID if you reuse this snippet:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ReadEduCloudProductionParameters",
      "Effect": "Allow",
      "Action": [
        "ssm:GetParameter",
        "ssm:GetParameters"
      ],
      "Resource": [
        "arn:aws:ssm:ap-southeast-1:ACCOUNT_ID:parameter/educloud/production/database-url",
        "arn:aws:ssm:ap-southeast-1:ACCOUNT_ID:parameter/educloud/production/jwt-secret"
      ]
    }
  ]
}
```

![IAM policy for the Elastic Beanstalk EC2 role](/images/workshop/04-eb-ec2-role-ssm-policy.png)

Later, create Elastic Beanstalk environment properties named `DATABASE_URL` and
`JWT_SECRET_KEY`, choose **Parameter Store** as their source, and paste the
matching parameter ARN.

The same EC2 role later receives separate least-privilege statements for S3
multipart object access, `s3:ListBucket` on the `courses/` prefix,
`ce:GetCostAndUsage`, `ce:GetCostForecast`, and `logs:FilterLogEvents`. These
are read/upload capabilities only; no billing or CloudWatch mutation permission
is required by the application dashboard.
