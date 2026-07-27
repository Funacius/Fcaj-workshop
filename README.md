# EduCloud Lite internship workshop

This is the Hugo source for the EduCloud Lite internship report and deployment
workshop. It is based on the FCJ internship template and `hugo-theme-learn`.

## Preview locally

Use Hugo Extended 0.134.3:

```powershell
.\preview.ps1
```

Open <http://localhost:1313>.

## Build

```powershell
.\build.ps1
```

The generated static website is written to `public/`. Do not commit that folder.
GitHub Actions builds and deploys the site to GitHub Pages after each push to
`main`.

`build.ps1` uses `/` as the local base URL, so `public/index.html` can also be
opened directly for a quick visual check. For normal editing, prefer
`preview.ps1` and `http://localhost:1313` because search and navigation behave
more like the deployed website.

The report content, bilingual navigation, workshop screenshots, blog posts, and
event evidence are maintained under `content/` and `static/images/`.
