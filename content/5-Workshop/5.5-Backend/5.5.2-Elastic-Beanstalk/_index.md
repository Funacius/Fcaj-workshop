---
title: "Elastic Beanstalk"
weight: 2
chapter: false
pre: "<b>5.5.2.</b>"
---

# Create Elastic Beanstalk

Create an Elastic Beanstalk environment:

- Tier: Web server environment.
- Platform: Python 3.12 on 64-bit Amazon Linux 2023.
- Environment type: Single instance.
- Instance type: `t3.micro` or `t3.small`.
- Public IPv4: enabled for the current simple design.
- Managed platform updates: disabled when using Basic health reporting.

Upload `educloud-backend.zip`.

After deployment, open the Elastic Beanstalk domain and `/docs`. Health should
be green before continuing.

In **Configuration**, enable **Instance log streaming to CloudWatch Logs** and
select a seven-day retention period for this workshop. The application setting
`AWS_CLOUDWATCH_LOG_GROUP` only selects the group that Admin Logs reads; it does
not create the group or turn on Elastic Beanstalk streaming.

![Elastic Beanstalk health Green](/images/workshop/05-elastic-beanstalk-green.png)
