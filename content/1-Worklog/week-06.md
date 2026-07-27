---
title: "Week 6 - Enrollment and learning progress"
menuTitle: "Week 6"
weight: 6
pre: "<b>1.6.</b>"
---

**Period:** July 13, 2026 - July 17, 2026

## Objectives

- Provide a public published-course catalog and enrollment flow.
- Build the Student Learning Page.
- Persist lesson-level progress and completion percentages.

## AWS Learning Phase

| Learning content | Reference | Application in EduCloud |
| --- | --- | --- |
| Frontend hosting, static website deployment, and routing behavior for single-page applications. | [Deploy Static Website on AWS](https://000057.awsstudygroup.com/) | Prepared the React application for later production hosting. |
| CDN behavior, cache settings, and private origin delivery. | [CloudFront with S3 Origin](https://000094.awsstudygroup.com/) | Planned how student learning resources could be delivered through CloudFront. |

## Work completed

| Date | Activity | Outcome |
| --- | --- | --- |
| Jul 13 | Built the catalog, search, course cards, and detail page. | Loaded published courses from Supabase. |
| Jul 14 | Implemented Student-only, idempotent enrollment with uniqueness constraints. | Prevented duplicate enrollments. |
| Jul 15 | Built the Learning Page with lesson playlist, video, notes, and materials. | Restricted full content to enrolled Students. |
| Jul 16 | Implemented complete/undo progress APIs and completion calculation. | Persisted per-Student lesson progress. |
| Jul 17 | Added My Learning, resume-first-incomplete behavior, and Instructor counts. | Enabled continuity and live reporting. |

## Achievements

- Public APIs expose outlines rather than private lesson assets.
- Student enrollments and progress persist in Supabase.
- Learning supports lesson navigation and completion.
- Dashboards use database data instead of fixed demo values.

## Project evidence

- [Enrollment service](https://github.com/Funacius/EduCloud/blob/main/backend/app/services/enrollment_service.py)
- [Progress service](https://github.com/Funacius/EduCloud/blob/main/backend/app/services/progress_service.py)
- [Learning Page](https://github.com/Funacius/EduCloud/blob/main/frontend/src/pages/LearningPage.tsx)
