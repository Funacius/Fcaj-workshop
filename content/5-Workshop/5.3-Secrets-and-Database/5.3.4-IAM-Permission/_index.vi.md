---
title: "Cấp quyền IAM"
weight: 4
chapter: false
pre: "<b>5.3.4.</b>"
---

# Cấp quyền IAM

Attach inline policy vào Elastic Beanstalk EC2 instance role. Nếu dùng lại đoạn
này, thay `ACCOUNT_ID` bằng account ID của bạn:

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

![IAM policy cho Elastic Beanstalk EC2 role](/images/workshop/04-eb-ec2-role-ssm-policy.png)

Sau đó, trong Elastic Beanstalk Environment properties, tạo `DATABASE_URL` và
`JWT_SECRET_KEY`, chọn source **Parameter Store** và dán ARN tương ứng.

Cùng EC2 role này sẽ nhận các statement least-privilege riêng cho S3 multipart,
`s3:ListBucket` giới hạn ở prefix `courses/`, `ce:GetCostAndUsage`,
`ce:GetCostForecast` và `logs:FilterLogEvents`. Dashboard chỉ cần quyền đọc và
upload; không cần quyền thay đổi Billing hoặc CloudWatch.
