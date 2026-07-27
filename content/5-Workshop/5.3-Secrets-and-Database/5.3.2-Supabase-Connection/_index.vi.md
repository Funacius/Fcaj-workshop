---
title: "Tạo Supabase connection"
weight: 2
chapter: false
pre: "<b>5.3.2.</b>"
---

# Tạo Supabase connection

1. Tạo Supabase project.
2. Mở **Connect**.
3. Chọn **Direct** và chọn **Session Pooler**.
4. Chọn type **URI**.
5. Copy connection string.
6. Đổi `postgresql://` thành `postgresql+psycopg2://`.
7. URL-encode password.
8. Thêm `?sslmode=require`.

Encode password mà không ghi vào shell history:

```powershell
$databasePassword = Read-Host "Database password"
[uri]::EscapeDataString($databasePassword)
Remove-Variable databasePassword
```

Giá trị cuối cùng có dạng:

```text
postgresql+psycopg2://USER:ENCODED_PASSWORD@POOLER_HOST:5432/postgres?sslmode=require
```

![Supabase Session Pooler](/images/workshop/02-supabase-session-pooler.png)

Supabase Table Editor có thể dùng để kiểm tra nhanh schema và dữ liệu mẫu. Bảng
`users` thể hiện các role cấp ứng dụng mà EduCloud Lite sử dụng.

![Supabase Table Editor](/images/workshop/02b-supabase-table-editor.png)

