---
title: "Week 8 - Final validation, report, and submission"
menuTitle: "Week 8"
weight: 8
pre: "<b>1.8.</b>"
---

**Period:** July 27, 2026 - July 31, 2026

**Submission deadline:** July 31, 2026

## Objectives

- Freeze the EduCloud Lite feature scope and avoid introducing risky changes before submission.
- Validate the live Student and Instructor journeys, AWS services, and public links.
- Complete the bilingual Hugo workshop, evidence, architecture diagram, and submission package.
- Submit the workshop no later than July 31, 2026.

## Final AWS and Project Review

| Review area | Validation activity | Submission evidence |
| --- | --- | --- |
| Identity and security | Review Cognito login, first-login password change, recovery, JWT exchange, IAM roles, and Parameter Store secrets. | Authentication screenshots, configuration notes, and source links. |
| Application hosting | Check Elastic Beanstalk health, Amplify deployment, CloudFront routing, CORS, and SPA rewrites. | Public EduCloud link and successful deployment status. |
| Storage and data | Verify private S3 delivery, Supabase connectivity, upload paths, and database-backed roles. | Architecture diagram and workshop screenshots. |
| Documentation | Review the proposal, worklog, blogs, events, workshop steps, self-assessment, and feedback pages in both languages. | Public Hugo report and GitHub repository. |

## Work plan and current progress

| Date | Activity | Target outcome |
| --- | --- | --- |
| Jul 27 | Correct the internship dates and worklog scope; complete the remaining report content and evidence. | Report reflects the actual June 1-August 15 internship period. |
| Jul 28 | Run the production checklist for login, course browsing, enrollment, learning, assessment, certificate, and Instructor authoring. | Critical demo journeys work through the public website. |
| Jul 29 | Review AWS architecture, screenshots, code snippets, repository instructions, and demo-account guidance. | Submission is understandable and reproducible by the mentor. |
| Jul 30 | Proofread the English and Vietnamese sites; check navigation, visited-page markers, images, links, and responsive layout. | Hugo report is consistent and has no placeholders or broken assets. |
| Jul 31 | Create the final submission package and send the live website, source repository, workshop link, and selected PDF files. | Workshop submitted before the deadline. |

## Current results

- EduCloud Lite is available through an independent Amplify URL.
- The FastAPI backend runs on Elastic Beanstalk and uses production secrets from Parameter Store.
- Cognito, Supabase, S3, CloudFront, and Amplify are connected to the deployed application.
- The architecture diagram, bilingual Hugo report, demo links, and mentor accounts are documented.
- Remaining work is limited to final validation, evidence review, proofreading, and submission.

## Submission links

- [EduCloud Lite live application](https://main.djk00b5qbck73.amplifyapp.com/courses)
- [EduCloud source repository](https://github.com/Funacius/EduCloud)
- [Public internship report](https://funacius.github.io/Fcaj-workshop/)
- [Hugo report repository](https://github.com/Funacius/Fcaj-workshop)

> No new project features are scheduled after July 31. The remaining internship time is reserved for AWS and First Cloud AI Journey learning.
