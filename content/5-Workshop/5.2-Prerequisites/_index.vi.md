---
title: "Chuẩn bị"
weight: 2
chapter: false
pre: "<b>5.2.</b>"
---

# Chuẩn bị

Cần có AWS account, GitHub repository, Supabase/PostgreSQL database, Git, Python
3.11+, Node.js 18+, AWS CLI v2 và PowerShell.

```powershell
git --version
python --version
node --version
npm --version
aws --version
git clone https://github.com/Funacius/EduCloud.git
Set-Location EduCloud
```

Workshop sử dụng Region Singapore `ap-southeast-1`. Parameter Store, Cognito,
Lambda, Elastic Beanstalk và S3 nên được tạo cùng Region.

> Lưu ý: Nên tạo AWS Budget trước khi bắt đầu. Không dùng bucket hệ thống của
> Elastic Beanstalk để lưu file khóa học; hãy tạo một S3 bucket riêng cho upload.
