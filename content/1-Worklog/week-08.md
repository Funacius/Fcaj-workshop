---
title: "Week 8 - Profiles, Instructor review, and Admin"
menuTitle: "Week 8"
weight: 8
pre: "<b>1.8.</b>"
---

**Period:** July 27, 2026 - July 31, 2026

## Objectives

- Store Student certificate identity data.
- Add an Admin-reviewed Instructor application workflow.
- Replace demo values with live administration dashboards.

## AWS Learning Phase

| Learning content | Reference | Application in EduCloud |
| --- | --- | --- |
| IAM role separation and administrative access control. | [Access Control with AWS IAM](https://000002.awsstudygroup.com/) | Modeled Admin review and Instructor promotion with explicit approval. |
| CloudWatch metrics, logs, service health, and monitoring views. | [AWS CloudWatch Workshop](https://000008.awsstudygroup.com/) | Designed Admin health and live dashboard indicators. |

## Work completed

| Date | Activity | Outcome |
| --- | --- | --- |
| Jul 27 | Added Student profiles for certificate name, birth date, organization, country, and bio. | Allowed identity completion before learning. |
| Jul 28 | Designed Instructor requests with pending/approved/rejected status and review audit. | Created a controlled promotion workflow. |
| Jul 29 | Built application, rejection-note, and resubmission UI. | Gave Students visibility into review status. |
| Jul 30 | Built live Admin metrics, review queue, and course oversight. | Replaced fixed dashboard values. |
| Jul 31 | Added traffic, latency, database, storage, and service health reporting. | Created an operational health page. |

## Achievements

- Separated certificate profile data from authentication.
- Required Admin approval before Instructor promotion.
- Required rejection notes and supported resubmission.
- Added live management and system-health dashboards.

## Project evidence

- [Profile routes](https://github.com/Funacius/EduCloud/blob/main/backend/app/routes/profile_routes.py)
- [Instructor request service](https://github.com/Funacius/EduCloud/blob/main/backend/app/services/instructor_request_service.py)
- [Admin Dashboard](https://github.com/Funacius/EduCloud/blob/main/frontend/src/pages/AdminDashboardPage.tsx)
- [Admin Health](https://github.com/Funacius/EduCloud/blob/main/frontend/src/pages/AdminHealthPage.tsx)
