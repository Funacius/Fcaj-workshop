---
title: "Đóng gói FastAPI"
weight: 1
chapter: false
pre: "<b>5.5.1.</b>"
---

# Đóng gói FastAPI

Tạo ZIP từ trong thư mục `backend` để `main.py`, `requirements.txt`, `Procfile`
và `app/` nằm ngay ở root của file ZIP:

```powershell
Set-Location backend
Compress-Archive `
  -Path app,scripts,main.py,requirements.txt,Procfile `
  -DestinationPath ..\educloud-backend.zip `
  -Force
Set-Location ..
```

`Procfile` dùng lệnh:

```text
web: uvicorn main:app --host 0.0.0.0 --port 8000
```

