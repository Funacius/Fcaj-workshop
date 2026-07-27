---
title: "Week 5 - Courses, lessons, and resources"
menuTitle: "Week 5"
weight: 5
pre: "<b>1.5.</b>"
---

**Period:** July 6, 2026 - July 10, 2026

## Objectives

- Enable Instructors to create and manage owned courses.
- Organize ordered lesson curricula.
- Support thumbnails, videos, and learning materials.

## AWS Learning Phase

| Learning content | Reference | Application in EduCloud |
| --- | --- | --- |
| S3 object storage, bucket privacy, and storing uploaded learning resources. | [Amazon S3 Security Best Practices](https://000069.awsstudygroup.com/) | Planned a private upload bucket for course images and lesson resources. |
| Amplify authentication and storage patterns using Cognito and S3. | [Amplify Authentication and Storage](https://000134.awsstudygroup.com/) | Prepared the frontend and backend to later move uploads from local storage to AWS S3. |

## Work completed

| Date | Activity | Outcome |
| --- | --- | --- |
| Jul 06 | Implemented Course models, schemas, services, CRUD routes, and ownership checks. | Instructors can mutate only owned courses. |
| Jul 07 | Built Instructor list/create/edit pages for course metadata, tags, price, and status. | Completed the Instructor course workspace. |
| Jul 08 | Implemented Lesson CRUD, order indexes, notes, video URLs, and material URLs. | Enabled ordered curricula. |
| Jul 09 | Built the Curriculum Editor with create/update/delete and validation. | Connected authoring UI to persistent data. |
| Jul 10 | Added local upload abstraction, thumbnail preview, and a draggable 4:3 crop editor. | Supported resources and 1200x900 thumbnails. |

## Achievements

- Completed owned Course and Lesson CRUD.
- Added Draft, Published, and Hidden states.
- Protected private curriculum content.
- Added drag, zoom, replace, and crop behavior for thumbnails.

## Project evidence

- [Course routes](https://github.com/Funacius/EduCloud/blob/main/backend/app/routes/course_routes.py)
- [Curriculum Editor](https://github.com/Funacius/EduCloud/blob/main/frontend/src/components/CourseCurriculumEditor.tsx)
- [Thumbnail Crop Editor](https://github.com/Funacius/EduCloud/blob/main/frontend/src/components/ThumbnailCropEditor.tsx)
