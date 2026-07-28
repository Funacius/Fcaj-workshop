---
title: "Tạo Elastic Beanstalk"
weight: 2
chapter: false
pre: "<b>5.5.2.</b>"
---

# Tạo Elastic Beanstalk

Tạo Elastic Beanstalk environment:

- Tier: Web server environment.
- Platform: Python 3.12 on 64-bit Amazon Linux 2023.
- Environment type: Single instance.
- Instance type: `t3.micro` hoặc `t3.small`.
- Public IPv4: enabled cho thiết kế demo đơn giản.
- Managed platform updates: tắt nếu dùng Basic health reporting.

Upload `educloud-backend.zip`.

Sau khi deploy, mở Elastic Beanstalk domain và `/docs`. Health nên trở về
**Green** trước khi tiếp tục.

Trong **Configuration**, bật **Instance log streaming to CloudWatch Logs** và
chọn retention 7 ngày cho workshop. Biến `AWS_CLOUDWATCH_LOG_GROUP` chỉ chọn log
group mà trang Admin Logs sẽ đọc; biến này không tự tạo log group hay bật log
streaming của Elastic Beanstalk.

![Elastic Beanstalk health Green](/images/workshop/05-elastic-beanstalk-green.png)
