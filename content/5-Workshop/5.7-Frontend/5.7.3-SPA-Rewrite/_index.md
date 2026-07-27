---
title: "SPA Rewrite"
weight: 3
chapter: false
pre: "<b>5.7.3.</b>"
---

# Add SPA Rewrite

Add a `200` rewrite that serves `/index.html` for application routes while
excluding real static assets. This allows URLs such as `/login`, `/profile`, and
`/instructor` to load after refresh.

![Amplify SPA rewrite rule](/images/workshop/08b-amplify-spa-rewrite.png)

Then return to Elastic Beanstalk and set:

```text
CORS_ORIGINS=https://YOUR_AMPLIFY_DOMAIN
```

Wait for the backend environment update, then redeploy Amplify if any frontend
variable changed.
