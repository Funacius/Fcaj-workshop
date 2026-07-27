---
title: "Parameter Store"
weight: 3
chapter: false
pre: "<b>5.3.3.</b>"
---

# Store Secrets in Parameter Store

Generate a JWT secret:

```powershell
$secretBytes = New-Object byte[] 64
[System.Security.Cryptography.RandomNumberGenerator]::Fill($secretBytes)
[Convert]::ToBase64String($secretBytes)
Remove-Variable secretBytes
```

Create two **SecureString** parameters in `ap-southeast-1`:

```text
/educloud/production/database-url
/educloud/production/jwt-secret
```

Do not paste the secret values into this workshop, screenshots, GitHub, frontend
variables, or the Elastic Beanstalk plain-text fields.

![AWS Systems Manager Parameter Store](/images/workshop/03-ssm-secure-parameters.png)

