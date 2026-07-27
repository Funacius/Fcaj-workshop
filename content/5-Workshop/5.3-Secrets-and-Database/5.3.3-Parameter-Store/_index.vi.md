---
title: "Lưu Parameter Store"
weight: 3
chapter: false
pre: "<b>5.3.3.</b>"
---

# Lưu secret vào Parameter Store

Tạo JWT secret:

```powershell
$secretBytes = New-Object byte[] 64
[System.Security.Cryptography.RandomNumberGenerator]::Fill($secretBytes)
[Convert]::ToBase64String($secretBytes)
Remove-Variable secretBytes
```

Tạo hai parameter type **SecureString** trong `ap-southeast-1`:

```text
/educloud/production/database-url
/educloud/production/jwt-secret
```

Không đưa database password hoặc JWT secret vào GitHub, frontend, ảnh chụp màn
hình hoặc plain text trong Elastic Beanstalk.

![AWS Systems Manager Parameter Store](/images/workshop/03-ssm-secure-parameters.png)

