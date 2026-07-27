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

![Elastic Beanstalk health Green](/images/workshop/05-elastic-beanstalk-green.png)

