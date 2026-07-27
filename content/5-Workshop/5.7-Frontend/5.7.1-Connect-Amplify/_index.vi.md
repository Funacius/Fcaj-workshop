---
title: "Kết nối Amplify"
weight: 1
chapter: false
pre: "<b>5.7.1.</b>"
---

# Kết nối GitHub với Amplify

Trong Amplify Hosting:

1. Chọn GitHub.
2. Chọn EduCloud repository và nhánh `main`.
3. Set monorepo app root là `frontend`.
4. Build command: `npm run build`.
5. Output directory: `dist`.

Repository có `amplify.yml`, chạy `npm ci` và `npm run build`.

![Amplify deploy thành công](/images/workshop/08-amplify-deployed.png)

