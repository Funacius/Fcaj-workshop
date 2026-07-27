---
title: "Connect Amplify"
weight: 1
chapter: false
pre: "<b>5.7.1.</b>"
---

# Connect GitHub to Amplify

In Amplify Hosting:

1. Choose GitHub.
2. Select the EduCloud repository and `main` branch.
3. Set the monorepo app root to `frontend`.
4. Build command: `npm run build`.
5. Output directory: `dist`.

The repository includes `amplify.yml`, which runs `npm ci` and `npm run build`.

![Amplify deployment succeeded](/images/workshop/08-amplify-deployed.png)

