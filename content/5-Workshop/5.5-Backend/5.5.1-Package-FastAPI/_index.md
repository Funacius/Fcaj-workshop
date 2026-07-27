---
title: "Package FastAPI"
weight: 1
chapter: false
pre: "<b>5.5.1.</b>"
---

# Package FastAPI

Create the ZIP from inside `backend` so `main.py`, `requirements.txt`, `Procfile`,
and `app/` are at the archive root:

```powershell
Set-Location backend
Compress-Archive `
  -Path app,scripts,main.py,requirements.txt,Procfile `
  -DestinationPath ..\educloud-backend.zip `
  -Force
Set-Location ..
```

The included `Procfile` starts:

```text
web: uvicorn main:app --host 0.0.0.0 --port 8000
```

