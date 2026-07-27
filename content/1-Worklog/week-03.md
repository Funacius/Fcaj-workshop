---
title: "Week 3 - Backend and database foundation"
menuTitle: "Week 3"
weight: 3
pre: "<b>1.3.</b>"
---

**Period:** June 22, 2026 - June 26, 2026

## Objectives

- Design the React, FastAPI, and PostgreSQL request flow.
- Connect FastAPI to Supabase PostgreSQL through SQLAlchemy.
- Establish models, schemas, services, and routes.
- Prepare reusable environment, migration, and seed tooling.

## AWS Learning Phase

| Learning content | Reference | Application in EduCloud |
| --- | --- | --- |
| IAM users, policies, roles, and least-privilege access. | [Access Control with AWS IAM](https://000002.awsstudygroup.com/) | Separated AWS infrastructure permissions from Student, Instructor, and Admin application roles. |
| VPC, subnets, routing, security groups, and how an application is accessed from the Internet. | [Amazon VPC Workshop](https://000003.awsstudygroup.com/) | Designed the network flow for backend and database access. |
| EC2, Amazon Linux, compute resources, and application hosting concepts. | [Introduction to Amazon EC2](https://000004.awsstudygroup.com/) | Understood the infrastructure layer behind Elastic Beanstalk. |
| Relational databases, PostgreSQL, SSL, backups, and network isolation. | [Amazon RDS Workshop](https://000005.awsstudygroup.com/) | Designed SQLAlchemy models and secure Supabase PostgreSQL connectivity. |

## Work completed

| Date | Activity | Outcome |
| --- | --- | --- |
| Jun 22 | Designed the React -> FastAPI -> PostgreSQL architecture and separated frontend, backend, and database responsibilities. | Produced the EduCloud request flow and technical stack. |
| Jun 23 | Implemented environment configuration, `.env.example`, and SQLAlchemy with the Supabase SSL pooler URI. | Kept secrets outside source control and established a stable database connection. |
| Jun 24 | Added User, Course, Lesson, Enrollment, and Progress models. | Represented core business relationships. |
| Jun 25 | Added Pydantic schemas, services, routers, and response helpers. | Established a consistent backend structure. |
| Jun 26 | Added startup creation, compatibility migrations, database checks, and development seeds. | Made new environments reproducible. |

## Achievements

- Connected FastAPI to Supabase with `psycopg2`.
- Established router → service → model separation.
- Added a safe database diagnostic script.
- Prepared Student, Instructor, and Admin development data.

## Project evidence

- [Database setup](https://github.com/Funacius/EduCloud/blob/main/backend/app/database.py)
- [Database migrations](https://github.com/Funacius/EduCloud/blob/main/backend/app/database_migrations.py)
- [Database diagnostic](https://github.com/Funacius/EduCloud/blob/main/backend/scripts/check_database.py)
