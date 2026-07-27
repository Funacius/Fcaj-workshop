---
title: "Secrets and Database"
weight: 3
chapter: false
pre: "<b>5.3.</b>"
---

# Configure Secrets and Database

EduCloud Lite needs PostgreSQL for users, courses, lessons, learning progress,
assessments, and certificates. The backend reads the database URL and JWT secret
from AWS Systems Manager Parameter Store instead of storing secrets in code.

## Contents

1. [Why Supabase](5.3.1-why-supabase/)
2. [Create the Supabase connection](5.3.2-supabase-connection/)
3. [Store secrets in Parameter Store](5.3.3-parameter-store/)
4. [Grant IAM permission](5.3.4-iam-permission/)

