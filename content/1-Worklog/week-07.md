---
title: "Week 7 - Final assessments and certificates"
menuTitle: "Week 7"
weight: 7
pre: "<b>1.7.</b>"
---

**Period:** July 20, 2026 - July 24, 2026

## Objectives

- Allow Instructors to configure final course assessments.
- Enforce timing, scoring, and attempt limits in the backend.
- Issue certificates only after lessons and assessment completion.

## AWS Learning Phase

| Learning content | Reference | Application in EduCloud |
| --- | --- | --- |
| CloudWatch metrics, logs, alarms, and dashboard concepts. | [AWS CloudWatch Workshop](https://000008.awsstudygroup.com/) | Planned how assessment and certificate flows could be monitored after deployment. |
| Modern application operation patterns: health checks, idempotency, and recoverable request handling. | [Explore AWS Services](https://cloudjourney.awsstudygroup.com/) | Designed idempotent certificate issuance and safer assessment submission rules. |

## Work completed

| Date | Activity | Outcome |
| --- | --- | --- |
| Jul 20 | Designed assessment settings, questions, and attempt tables. | Persisted configuration and attempt history. |
| Jul 21 | Built an editor for timer, pass mark, attempts, publication, and dynamic options. | Enabled flexible assessment authoring. |
| Jul 22 | Added single, all-correct, and any-correct modes plus option drag-and-drop. | Supported multiple grading strategies. |
| Jul 23 | Built the timed Student page with question navigator, answered state, and auto-submit. | Improved assessment usability and review. |
| Jul 24 | Added backend scoring/deadlines/eligibility and idempotent certificate issuance with print UI. | Enforced completion rules end to end. |

## Achievements

- Backend-enforced timers and attempt limits.
- Up to twelve options and multiple correct answers.
- Clickable question navigator and previous/next controls.
- Printable certificate issued only after a passing completion.

## Project evidence

- [Assessment service](https://github.com/Funacius/EduCloud/blob/main/backend/app/services/assessment_service.py)
- [Assessment Editor](https://github.com/Funacius/EduCloud/blob/main/frontend/src/components/AssessmentEditor.tsx)
- [Assessment Page](https://github.com/Funacius/EduCloud/blob/main/frontend/src/pages/AssessmentPage.tsx)
- [Certificate Page](https://github.com/Funacius/EduCloud/blob/main/frontend/src/pages/CertificatePage.tsx)
