---
title: "Supabase Connection"
weight: 2
chapter: false
pre: "<b>5.3.2.</b>"
---

# Create the Supabase Connection

1. Create a Supabase project.
2. Open **Connect**.
3. Choose **Direct** and select **Session Pooler**.
4. Set type to **URI**.
5. Copy the connection string.
6. Change `postgresql://` to `postgresql+psycopg2://`.
7. URL-encode the password.
8. Add `?sslmode=require`.

Encode a password without printing it into shell history:

```powershell
$databasePassword = Read-Host "Database password"
[uri]::EscapeDataString($databasePassword)
Remove-Variable databasePassword
```

The final value has this form:

```text
postgresql+psycopg2://USER:ENCODED_PASSWORD@POOLER_HOST:5432/postgres?sslmode=require
```

![Supabase Session Pooler](/images/workshop/02-supabase-session-pooler.png)

The Supabase Table Editor can verify that EduCloud tables and sample records
exist. The `users` table shows the application-level roles used by EduCloud Lite.

![Supabase Table Editor](/images/workshop/02b-supabase-table-editor.png)

