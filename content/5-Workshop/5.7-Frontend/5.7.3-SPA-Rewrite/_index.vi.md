---
title: "SPA rewrite"
weight: 3
chapter: false
pre: "<b>5.7.3.</b>"
---

# Thêm SPA rewrite

Thêm rewrite `200` để serve `/index.html` cho application routes nhưng vẫn loại
trừ static assets thật. Nhờ vậy các route như `/login`, `/profile`, `/instructor`
không bị 404 khi refresh.

![Amplify SPA rewrite rule](/images/workshop/08b-amplify-spa-rewrite.png)

Sau đó quay lại Elastic Beanstalk và set:

```text
CORS_ORIGINS=https://YOUR_AMPLIFY_DOMAIN
```

Chờ backend environment update xong, rồi redeploy Amplify nếu có frontend
variable thay đổi.
